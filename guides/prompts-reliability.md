## Reliability

## 可靠性

We have seen already how effective well-crafted prompts can be for various tasks using techniques like few-shot learning. As we think about building real-world applications on top of LLMs, it becomes crucial to think about the reliability of these language models. This guide focuses on demonstrating effective prompting techniques to improve the reliability of LLMs like GPT-3. Some topics of interest include generalizability, calibration, biases, social biases, and factuality to name a few.

我們已經看到了，使用少量樣本學習等技術，精心製作的提示對於各種任務的效果有多麼有效。當我們考慮在LLMs之上建構現實世界的應用時，思考這些語言模型的可靠性變得至關重要。本指南重點介紹了有效的提示技巧，以提高像GPT-3這樣的LLMs的可靠性。一些感興趣的主題包括通用性、校準、偏見、社會偏見和事實性等。

**Note that this section is under heavy development.**

請注意，此部分正在大力開發中。

Topics:

主題：

- [Factuality](#factuality)
- [Biases](#biases)
- ...


---

- [真實性](#factuality)
- [偏見](#biases)
- ...

---

## Factuality

## 真實性

LLMs have a tendency to generate responses that sounds coherent and convincing but can sometimes be made up. Improving prompts can help improve the model to generate more accurate/factual responses and reduce the likelihood to generate inconsistent and made up responses.

LLMs有時會產生聽起來連貫且令人信服但有時是虛構的回覆。改進提示可以幫助改善模型以產生更準確/真實的回覆，並減少產生不一致和虛構回覆的可能性。

Some solutions might include:

一些解決方案可能包括：

- provide ground truth (e.g., related article paragraph or Wikipedia entry) as part of context to reduce the likelihood of the model producing made up text.
- configure the model to produce less diverse responses by decreasing the probability parameters and instructing it to admit (e.g., "I don't know") when it doesn't know the answer.
- provide in the prompt a combination of examples of questions and responses that it might know about and not know about


---

- 提供真實的背景資訊（例如相關文章段落或維基百科條目），以減少模型產生虛假文字的可能性。
- 透過降低機率引數並指示模型在不知道答案時承認（例如，“我不知道”）來配置模型以產生較少多樣化的響應。
- 在提示中提供問題和答案的組合示範，包括可能已知和未知的內容。

Let's look at a simple example:

讓我們來看一個簡單的例子：

*Prompt:*

*提示:*

```
Q: What is an atom?
A: An atom is a tiny particle that makes up everything.

Q: Who is Alvan Muntz?
A: ?

Q: What is Kozar-09?
A: ? Q:

How many moons does Mars have?
A: Two, Phobos and Deimos.

Q: Who is Neto Beto Roberto?
```

*Output:*

*輸出:*

```
A: ?
```

I made up the name "Neto Beto Roberto" so the model is correct in this instance. Try to change the question a bit and see if you can get it to work. There are different ways you can improve this further based on all that you have learned so far.

我編造了“Neto Beto Roberto”這個名字，所以在這種情況下模型是正確的。試著稍微改變一下問題，看看能否讓它起作用。根據你到目前為止所學到的一切，有不同的方法可以進一步改進它。

---

## Biases

## 偏見

LLMs can produce problematic generations that can potentially be harmful and display biases that could deteriorate the performance of the model on downstream tasks. Some of these can be mitigates through effective prompting strategies but might require more advanced solutions like moderation and filtering.

LLMs 可能會產生有問題的生成物，這些生成物可能會有害，並顯示出可能會降低模型在下游任務中的效能的偏見。其中一些可以透過有效的提示策略來緩解，但可能需要更進階的解決方案，如調節和過濾。

### Distribution of Exemplars

### 樣本分佈

When performing few-shot learning, does the distribution of the exemplars affect the performance of the model or bias the model in some way? We can perform a simple test here.

當進行少樣本學習時，樣本的分佈是否會影響模型的效能或以某種方式對模型產生偏見？我們可以在這裡進行一個簡單的測試。

*Prompt:*

*提示:*

```
Q: I just got the best news ever!
A: Positive

Q: We just got a raise at work!
A: Positive

Q: I'm so proud of what I accomplished today.
A: Positive

Q: I'm having the best day ever!
A: Positive

Q: I'm really looking forward to the weekend.
A: Positive

Q: I just got the best present ever!
A: Positive

Q: I'm so happy right now.
A: Positive

Q: I'm so blessed to have such an amazing family.
A: Positive

Q: The weather outside is so gloomy.
A: Negative

Q: I just got some terrible news.
A: Negative

Q: That left a sour taste.
A:
```

*Output:*

*輸出:*

```
Negative
```

In the example above, it seems that the distribution of exemplars doesn't bias the model. This is good. Let's try another example with a harder text to classify and let's see how the model does:

在上面的例子中，似乎樣本的分佈並沒有對模型產生偏差。這很好。讓我們嘗試另一個更難分類的文字例子，看看模型的表現如何：

*Prompt:*

*提示:*

```
Q: The food here is delicious!
A: Positive

Q: I'm so tired of this coursework.
A: Negative

Q: I can't believe I failed the exam.
A: Negative

Q: I had a great day today!
A: Positive

Q: I hate this job.
A: Negative

Q: The service here is terrible.
A: Negative

Q: I'm so frustrated with my life.
A: Negative

Q: I never get a break.
A: Negative

Q: This meal tastes awful.
A: Negative

Q: I can't stand my boss.
A: Negative

Q: I feel something.
A:
```

*Output:*

*輸出:*

```
Negative
```

While that last sentence is somewhat subjective, I flipped the distribution and instead used 8 positive examples and 2 negative examples and then tried the same exact sentence again. Guess what the model responded? It responded "Positive". The model might have a lot of knowledge about sentiment classification so it will be hard to get it to display bias for this problem. The advice here is to avoid skewing the distribution and instead provide more balanced number of examples for each label. For harder tasks where the model doesn't have too much knowledge of, it will likely struggle more.

在某種程度上，最後一句話有點主觀性，我翻轉了分佈，使用了8個正面例子和2個負面例子，然後再次嘗試了完全相同的句子。你猜模型的回應是什麼？它回答“積極”。模型可能對情感分類有很多知識，因此很難讓它顯示出這個問題的偏見。建議避免偏斜分佈，而是為每個標籤提供更平衡的例子數量。對於模型沒有太多知識的更難的任務，它可能會更加困難。

### Order of Exemplars

### 示範順序

When performing few-shot learning, does the order affect the performance of the model or bias the model in some way?

在進行少樣本學習時，順序是否會影響模型的效能或以某種方式引入偏差？

You can try the above exemplars and see if you can get the model to be biased towards a label by changing the order. The advice is to randomly order exemplars. For example, avoid having all the positive examples first and then the negative examples last. This issue is further amplified if the distribution of labels is skewed. Always ensure to experiment a lot to reduce this type of biasness.

你可以嘗試上述示範，看看是否可以透過更改順序使模型偏向某個標籤。建議隨機排序示範。例如，避免首先放置所有正面示範，然後放置所有負面示範。如果標籤分佈不均，這個問題會進一步放大。始終確保進行大量實驗以減少這種偏差。

---

Other upcoming topics:

其他即將到來的主題：

- Perturbations
- Spurious Correlation
- Domain Shift
- Toxicity
- Hate speech / Offensive content
- Stereotypical bias
- Gender bias
- Coming soon!
- Red Teaming


---

- 擾動
- 偽相關性
- 領域偏移
- 毒性
- 仇恨言論 / 冒犯內容
- 刻板印象偏見
- 性別偏見
- 即將推出！
- 紅隊測試

---

## References

## 參考文獻

- [Constitutional AI: Harmlessness from AI Feedback](https://arxiv.org/abs/2212.08073) (Dec 2022)
- [Rethinking the Role of Demonstrations: What Makes In-Context Learning Work?](https://arxiv.org/abs/2202.12837) (Oct 2022)
- [Prompting GPT-3 To Be Reliable](https://arxiv.org/abs/2210.09150) (Oct 2022)
- [On the Advance of Making Language Models Better Reasoners](https://arxiv.org/abs/2206.02336) (Jun 2022)
- [Unsolved Problems in ML Safety](https://arxiv.org/abs/2109.13916) (Sep 2021)
- [Red Teaming Language Models to Reduce Harms: Methods, Scaling Behaviors, and Lessons Learned](https://arxiv.org/abs/2209.07858) (Aug 2022)
- [StereoSet: Measuring stereotypical bias in pretrained language models](https://aclanthology.org/2021.acl-long.416/) (Aug 2021)
- [Calibrate Before Use: Improving Few-Shot Performance of Language Models](https://arxiv.org/abs/2102.09690v2) (Feb 2021)
- [Techniques to improve reliability - OpenAI Cookbook](https://github.com/openai/openai-cookbook/blob/main/techniques_to_improve_reliability.md)


---

- [Constitutional AI: Harmlessness from AI Feedback](https://arxiv.org/abs/2212.08073) (2022年12月)
- [Rethinking the Role of Demonstrations: What Makes In-Context Learning Work?](https://arxiv.org/abs/2202.12837) (2022年10月)
- [Prompting GPT-3 To Be Reliable](https://arxiv.org/abs/2210.09150) (2022年10月)
- [On the Advance of Making Language Models Better Reasoners](https://arxiv.org/abs/2206.02336) (2022年6月)
- [Unsolved Problems in ML Safety](https://arxiv.org/abs/2109.13916) (2021年9月)
- [Red Teaming Language Models to Reduce Harms: Methods, Scaling Behaviors, and Lessons Learned](https://arxiv.org/abs/2209.07858) (2022年8月)
- [StereoSet: Measuring stereotypical bias in pretrained language models](https://aclanthology.org/2021.acl-long.416/) (2021年8月)
- [Calibrate Before Use: Improving Few-Shot Performance of Language Models](https://arxiv.org/abs/2102.09690v2) (2021年2月)
- [Techniques to improve reliability - OpenAI Cookbook](https://github.com/openai/openai-cookbook/blob/main/techniques_to_improve_reliability.md)

---

[Previous Section (Adversarial Prompting)](./prompts-adversarial.md)

[前一節（對抗提示）](./prompts-adversarial.md)

[Next Section (Miscellaneous)](./prompts-miscellaneous.md)

[下一節（雜項）](./prompts-miscellaneous.md)

