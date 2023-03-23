# Advanced Prompting

# 進階提示

By this point, it should be obvious that it helps to improve prompts to get better results on different tasks. That's the whole idea behind prompt engineering.

到了這個時候，改進提示以獲得不同任務的更好結果是有幫助的，這就是提示工程背後的整個理念。

While those examples were fun, let's cover a few concepts more formally before we jump into more advanced concepts.

雖然那些例子很有趣，但在我們深入探討更進階的概念之前，讓我們正式地介紹一些概念。

Topics:

主題：

- [Advanced Prompting](#advanced-prompting)
- [進階提示](#進階提示)
  - [Zero-Shot Prompting](#zero-shot-prompting)
  - [零樣本提示](#零樣本提示)
  - [Few-Shot Prompting](#few-shot-prompting)
  - [少量激勵](#少量激勵)
    - [Limitations of Few-shot Prompting](#limitations-of-few-shot-prompting)
    - [Few-shot Prompting 的限制](#few-shot-prompting-的限制)
  - [Chain-of-Thought Prompting](#chain-of-thought-prompting)
  - [關聯思考提示](#關聯思考提示)
  - [Zero-Shot CoT](#zero-shot-cot)
  - [零樣本CoT](#零樣本cot)
  - [Self-Consistency](#self-consistency)
  - [自洽性](#自洽性)
  - [Generated Knowledge Prompting](#generated-knowledge-prompting)
  - [產生知識提示](#產生知識提示)
  - [Automatic Prompt Engineer (APE)](#automatic-prompt-engineer-ape)
  - [自動提示工程師 (APE)](#自動提示工程師-ape)


---

- [zero-shot提示](#zero-shot提示)
- [few-shot提示](#few-shot提示)
- [思維鏈提示](#思維鏈提示)
- [zero-shot思維鏈](#zero-shot思維鏈)
- [自我一致性](#自我一致性)
- [產生知識提示](#產生知識提示)
- [自動提示工程師](#自動提示工程師-APE)

---

## Zero-Shot Prompting

## 零樣本提示

LLMs today trained on large amounts of data and tuned to follow instructions, are capable of performing tasks zero-shot. We tried a few zero-shot examples in the previous section. Here is one of the examples we used:

今天的LLMs在大量的資料訓練和調整後，能夠執行zero-shot任務。我們在前一節中嘗試了一些zero-shot示範。這是我們使用的其中一個示範：

*Prompt:*

*提示:*

```
Classify the text into neutral, negative, or positive.

Text: I think the vacation is okay.
Sentiment:
```

*Output:*

*輸出:*

```
Neutral
```

Note that in the prompt above we didn't provide the model with any examples -- that's the zero-shot capabilities at work. When zero-shot doesn't work, it's recommended to provide demonstrations or examples in the prompt. Below we discuss the approach known as few-shot prompting.

請注意，上面的提示中我們沒有向模型提供任何示範——這就是零樣本能力在發揮作用。當零樣本無法工作時，建議在提示中提供示範或示範。下面我們將討論稱為少樣本提示的方法。

---

## Few-Shot Prompting

## 少量激勵

While large-language models already demonstrate remarkable zero-shot capabilities, they still fall short on more complex tasks when using the zero-shot setting. To improve on this, few-shot prompting is used as a technique to enable in-context learning where we provide demonstrations in the prompt to steer the model to better performance. The demonstrations serve as conditioning for subsequent examples where we would like the model to generate a response.

雖然大型語言模型已經展現出了非凡的zero-shot能力，但在使用zero-shot設定時，它們仍然在更複雜的任務上表現不佳。為了改善這一點，採用了少量提示的技術，以實現上下文學習，在提示中提供示範以引導模型實現更好的效能。這些示範作為後續示範的條件，我們希望模型能夠產生回應。

Let's demonstrate few-shot prompting via an example that was presented by [Brown et al. 2020](https://arxiv.org/abs/2005.14165). In the example, the task is to correctly use a new word in a sentence.

讓我們透過 [Brown et al. 2020](https://arxiv.org/abs/2005.14165) 提出的一個例子來示範 few-shot prompting。在這個例子中，任務是在句子中正確使用一個新單詞。

*Prompt:*

*提示:*

```
A "whatpu" is a small, furry animal native to Tanzania. An example of a sentence that uses
the word whatpu is:
We were traveling in Africa and we saw these very cute whatpus.
To do a "farduddle" means to jump up and down really fast. An example of a sentence that uses
the word farduddle is:
```

*Output:*

*輸出:*

```
When we won the game, we all started to farduddle in celebration.
```

We can observe that the model has somehow learned how to perform the task by providing it with just one example (i.e., 1-shot). For more difficult tasks, we can experiment with increasing the demonstrations (e.g., 3-shot, 5-shot, 10-shot, etc.).

我們可以觀察到，該模型透過僅提供一個示範（即1-shot）已經學會了如何執行任務。對於更困難的任務，我們可以嘗試增加示範次數（例如3-shot、5-shot、10-shot等）。

Following the findings from [Min et al. (2022)](https://arxiv.org/abs/2202.12837), here are a few more tips about demonstrations/exemplars when doing few-shot:

根據[Min等人（2022）](https://arxiv.org/abs/2202.12837)的研究結果，以下是在進行少樣本學習時關於示範/示範的一些額外提示：

- "the label space and the distribution of the input text specified by the demonstrations are both important (regardless of whether the labels are correct for individual inputs)"
- the format you use also plays a key role in performance, even if you just use random labels, this is much better than no labels at all.
- additional results show that selecting random labels from a true distribution of labels (instead of a uniform distribution) also helps.


---

- "標籤空間和示範中指定的輸入文字的分佈都很重要（無論標籤對於個別輸入是否正確）"
- "即使您只使用隨機標籤，您使用的格式也對效能起著關鍵作用，這比根本不使用標籤要好得多。"
- "額外的結果顯示，從真實標籤分佈（而不是均勻分佈）中選擇隨機標籤也有所幫助。"

Let's try out a few examples. Let's first try an example with random labels (meaning the labels Negative and Positive are randomly assigned to the inputs):

讓我們試試幾個例子。首先，讓我們嘗試一個帶有隨機標籤的例子（意思是將標籤“負面”和“正面”隨機分配給輸入）。

*Prompt:*

*提示:*

```
This is awesome! // Negative
This is bad! // Positive
Wow that movie was rad! // Positive
What a horrible show! //
```

*Output:*

*輸出:*

```
Negative
```

We still get the correct answer, even though the labels have been randomized. Note that we also kept the format, which helps too. In fact, with further experimentation, it seems the newer GPT models we are experimenting with are becoming more robust to even random formats. Example:

即使標籤已經被隨機化，我們仍然得到了正確的答案。請注意，我們也保留了格式，這也有所幫助。實際上，透過進一步的實驗，似乎我們正在進行實驗的新GPT模型甚至對隨機格式變得更加強大。例如：

*Prompt:*

*提示:*

```
Positive This is awesome!
This is bad! Negative
Wow that movie was rad!
Positive
What a horrible show! --
```

*Output:*

*輸出:*

```
Negative
```

There is no consistency in the format above but the model still predicted the correct label. We have to conduct a more thorough analysis to confirm if this holds for different and more complex tasks, including different variations of prompts.

上述格式不一致，但模型仍然預測出正確的標籤。我們需要進行更全面的分析，以確認這是否適用於不同和更複雜的任務，包括不同變化的提示。

### Limitations of Few-shot Prompting

### Few-shot Prompting 的限制

Standard few-shot prompting works well for many tasks but is still not a perfect technique, especially when dealing with more complex reasoning tasks. Let's demonstrate why this is the case. Do you recall the previous example where we provided the following task:

標準的少量激勵技術對許多任務都有效，但在處理更複雜的推理任務時仍不是一種完美的技術。讓我們來示範一下為什麼會這樣。您還記得我們之前提供的以下任務嗎：

```
The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1.

A:
```

If we try this again, the model outputs the following:

如果我們再試一次，模型將輸出以下內容：

```
Yes, the odd numbers in this group add up to 107, which is an even number.
```

This is not the correct response, which not only highlights the limitations of these systems but that there is a need for more advanced prompt engineering.

這不是正確的回應，不僅凸顯了這些系統的限制，也需要更先進的提示工程。

Let's try to add some examples to see if few-shot prompting improves the results.

讓我們嘗試新增一些例子，看看少量提示是否可以改善結果。

*Prompt:*

*提示:*

```
The odd numbers in this group add up to an even number: 4, 8, 9, 15, 12, 2, 1.
A: The answer is False.

The odd numbers in this group add up to an even number: 17,  10, 19, 4, 8, 12, 24.
A: The answer is True.

The odd numbers in this group add up to an even number: 16,  11, 14, 4, 8, 13, 24.
A: The answer is True.

The odd numbers in this group add up to an even number: 17,  9, 10, 12, 13, 4, 2.
A: The answer is False.

The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1.
A:
```

*Output:*

*輸出:*

```
The answer is True.
```

That didn't work. It seems like few-shot prompting is not enough to get reliable responses for this type of reasoning problem. The example above provides basic information on the task. If you take a closer look, the type of task we have introduced involves a few more reasoning steps. In other words, it might help if we break the problem down into steps and demonstrate that to the model. More recently, [chain-of-thought (CoT) prompting](https://arxiv.org/abs/2201.11903) has been popularized to address more complex arithmetic, commonsense, and symbolic reasoning tasks.

那行不通。似乎少量提示不足以獲得此類推理問題的可靠響應。上面的示範提供了有關任務的基本資訊。如果您仔細觀察，我們引入的任務型別涉及更多的推理步驟。換句話說，如果我們將問題分解成步驟並向模型示範，可能會有所幫助。最近，[思維鏈 (CoT)提示](https://arxiv.org/abs/2201.11903)已經流行起來，以解決更複雜的算術、常識和符號推理任務。

Overall, it seems that providing examples is useful for solving some tasks. When zero-shot prompting and few-shot prompting are not sufficient, it might mean that whatever was learned by the model isn't enough to do well at the task. From here it is recommended to start thinking about fine-tuning your models or experimenting with more advanced prompting techniques. Up next we talk about one of the popular prompting techniques called chain-of-thought prompting which has gained a lot of popularity.

總的來說，提供示範對於解決某些任務是有用的。當zero-shot提示和少量提示不足時，這可能意味著模型學到的內容不足以在任務上表現良好。從這裡開始，建議開始考慮微調模型或嘗試更進階的提示技術。接下來，我們將討論一種流行的提示技術，稱為思維鏈提示，它已經獲得了很多的關注。

---

## Chain-of-Thought Prompting

## 關聯思考提示

Introduced in [Wei et al. (2022)](https://arxiv.org/abs/2201.11903), chain-of-thought (CoT) prompting enables complex reasoning capabilities through intermediate reasoning steps. You can combine it with few-shot prompting to get better results on more complex tasks that require reasoning before responding.

引入自[Wei et al. (2022)](https://arxiv.org/abs/2201.11903)的思路鏈（CoT）提示，透過中間推理步驟實現複雜的推理能力。您可以將其與少量提示結合使用，以獲得更好的結果，以應對需要在回答之前進行推理的更複雜任務。

*Prompt:*

*提示:*

```
The odd numbers in this group add up to an even number: 4, 8, 9, 15, 12, 2, 1.
A: Adding all the odd numbers (9, 15, 1) gives 25. The answer is False.

The odd numbers in this group add up to an even number: 17,  10, 19, 4, 8, 12, 24.
A: Adding all the odd numbers (17, 19) gives 36. The answer is True.

The odd numbers in this group add up to an even number: 16,  11, 14, 4, 8, 13, 24.
A: Adding all the odd numbers (11, 13) gives 24. The answer is True.

The odd numbers in this group add up to an even number: 17,  9, 10, 12, 13, 4, 2.
A: Adding all the odd numbers (17, 9, 13) gives 39. The answer is False.

The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1.
A:
```

*Output:*

*輸出:*

```
Adding all the odd numbers (15, 5, 13, 7, 1) gives 41. The answer is False.
```

Wow! We can see a perfect result when we provided the reasoning step. We can solve this task by providing even fewer examples, i.e., just one example seems enough:

哇！當我們提供了推理步驟時，我們可以看到完美的結果。我們可以透過提供更少的示範來解決這個任務，即只需一個示範似乎就足夠了：

*Prompt:*

*提示:*

```
The odd numbers in this group add up to an even number: 4, 8, 9, 15, 12, 2, 1.
A: Adding all the odd numbers (9, 15, 1) gives 25. The answer is False.

The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1.
A:
```

*Output:*

*輸出:*

```
Adding all the odd numbers (15, 5, 13, 7, 1) gives 41. The answer is False.
```

Keep in mind that the authors claim that this is an emergent ability that arises with sufficiently large language models.

請記住，作者聲稱這是一種新興的能力，它隨著足夠大的語言模型而出現。

---

## Zero-Shot CoT

## 零樣本CoT

One recent idea that came out more recently is the idea of [zero-shot CoT](https://arxiv.org/abs/2205.11916) (Kojima et al. 2022) that essentially involves adding "Let's think step by step" to the original prompt. Let's try a simple problem and see how the model performs:

最近提出的一個新想法是[zero-shot CoT](https://arxiv.org/abs/2205.11916)（Kojima等人，2022年），其基本上是在原始提示中新增“讓我們一步一步地思考”。讓我們試著解決一個簡單的問題，看看模型的表現如何：

*Prompt:*

*提示:*

```
I went to the market and bought 10 apples. I gave 2 apples to the neighbor and 2 to the repairman. I then went and bought 5 more apples and ate 1. How many apples did I remain with?
```

*Output:*

*輸出:*

```
11 apples
```

The answer is incorrect! Now Let's try with the special prompt.

答案不正確！現在讓我們嘗試使用特殊提示。

*Prompt:*

*提示:*

```
I went to the market and bought 10 apples. I gave 2 apples to the neighbor and 2 to the repairman. I then went and bought 5 more apples and ate 1. How many apples did I remain with?

Let's think step by step.
```

*Output:*

*輸出:*

```
First, you started with 10 apples.
You gave away 2 apples to the neighbor and 2 to the repairman, so you had 6 apples left.
Then you bought 5 more apples, so now you had 11 apples.
Finally, you ate 1 apple, so you would remain with 10 apples.
```

It's impressive that this simple prompt is effective at this task. This is particularly useful where you don't have too many examples to use in the prompt.

這個簡單的提示能夠有效地完成這個任務，這真是令人印象深刻。這在你沒有太多的提示例子可用時特別有用。

---

## Self-Consistency

## 自洽性

Perhaps one of the more advanced techniques out there for prompt engineering is self-consistency. Proposed by [Wang et al. (2022)](https://arxiv.org/pdf/2203.11171.pdf), self-consistency aims "to replace the naive greedy decoding used in chain-of-thought prompting". The idea is to sample multiple, diverse reasoning paths through few-shot CoT, and use the generations to select the most consistent answer. This helps to boost the performance of CoT prompting on tasks involving arithmetic and commonsense reasoning.

或許在提示工程中，更高級的技術之一是自洽性。由 [Wang et al. (2022)](https://arxiv.org/pdf/2203.11171.pdf) 提出，自洽性旨在「取代鏈式思考提示中使用的天真貪婪解碼」。其想法是透過少量樣本的多樣化推理路徑進行抽樣，並使用產生的結果選擇最一致的答案。這有助於提高鏈式思考提示在涉及算術和常識推理的任務中的效能。

Let's try the following example for arithmetic reasoning:

讓我們嘗試以下算術推理的例子：

*Prompt:*

*提示:*

```
When I was 6 my sister was half my age. Now
I’m 70 how old is my sister?
```

*Output:*

*輸出:*

```
35
```

The output is wrong! How may we improve this with self-consistency? Let's try it out. We will use the few-shot exemplars from Wang et al. 2022 (Table 17):

輸出結果是錯誤的！我們如何透過自洽性來改進它？讓我們試試看。我們將使用王等人的2022年少量樣本示範（表17）：

*Prompt:*

*提示:*

```
Q: There are 15 trees in the grove. Grove workers will plant trees in the grove today. After they are done,
there will be 21 trees. How many trees did the grove workers plant today?
A: We start with 15 trees. Later we have 21 trees. The difference must be the number of trees they planted.
So, they must have planted 21 - 15 = 6 trees. The answer is 6.

Q: If there are 3 cars in the parking lot and 2 more cars arrive, how many cars are in the parking lot?
A: There are 3 cars in the parking lot already. 2 more arrive. Now there are 3 + 2 = 5 cars. The answer is 5.

Q: Leah had 32 chocolates and her sister had 42. If they ate 35, how many pieces do they have left in total?
A: Leah had 32 chocolates and Leah’s sister had 42. That means there were originally 32 + 42 = 74
chocolates. 35 have been eaten. So in total they still have 74 - 35 = 39 chocolates. The answer is 39.

Q: Jason had 20 lollipops. He gave Denny some lollipops. Now Jason has 12 lollipops. How many lollipops
did Jason give to Denny?
A: Jason had 20 lollipops. Since he only has 12 now, he must have given the rest to Denny. The number of
lollipops he has given to Denny must have been 20 - 12 = 8 lollipops. The answer is 8.

Q: Shawn has five toys. For Christmas, he got two toys each from his mom and dad. How many toys does
he have now?
A: He has 5 toys. He got 2 from mom, so after that he has 5 + 2 = 7 toys. Then he got 2 more from dad, so
in total he has 7 + 2 = 9 toys. The answer is 9.

Q: There were nine computers in the server room. Five more computers were installed each day, from
monday to thursday. How many computers are now in the server room?
A: There are 4 days from monday to thursday. 5 computers were added each day. That means in total 4 * 5 =
20 computers were added. There were 9 computers in the beginning, so now there are 9 + 20 = 29 computers.
The answer is 29.

Q: Michael had 58 golf balls. On tuesday, he lost 23 golf balls. On wednesday, he lost 2 more. How many
golf balls did he have at the end of wednesday?
A: Michael initially had 58 balls. He lost 23 on Tuesday, so after that he has 58 - 23 = 35 balls. On
Wednesday he lost 2 more so now he has 35 - 2 = 33 balls. The answer is 33.

Q: Olivia has $23. She bought five bagels for $3 each. How much money does she have left?
A: She bought 5 bagels for $3 each. This means she spent 5

Q: When I was 6 my sister was half my age. Now I’m 70 how old is my sister?
A:
```

*Output 1:*

*輸出 1:*

```
When I was 6 my sister was half my age, so she was 3. Now I am 70, so she is 70 - 3 = 67. The answer is 67.
```

*Output 2:*

*輸出 2:*

```
When the narrator was 6, his sister was half his age, which is 3. Now that the narrator is 70, his sister would be 70 - 3 = 67 years old. The answer is 67.
```

*Output 3:*

*輸出 3:*

```
When I was 6 my sister was half my age, so she was 3. Now I am 70, so she is 70/2 = 35. The answer is 35.
```

Computing for the final answer involves a few steps (check out the paper for the details) but for the sake of simplicity, we can see that there is already a majority answer emerging so that would essentially become the final answer.

計算最終答案需要幾個步驟（詳細內容請參閱論文），但為了簡單起見，我們可以看到已經出現了大多數的答案，因此這基本上就成為了最終答案。

---

## Generated Knowledge Prompting

## 產生知識提示

LLMs continue to be improved and one popular technique includes the ability to incorporate knowledge or information to help the model make more accurate predictions.

LLMs持續改進，其中一種流行的技術包括能夠整合知識或資訊，以幫助模型做出更準確的預測。

Using a similar idea, can the model also be used to generate knowledge before making a prediction? That's what is attempted in the paper by [Liu et al. 2022](https://arxiv.org/pdf/2110.08387.pdf) -- generate knowledge to be used as part of the prompt. In particular, how helpful is this for tasks such as commonsense reasoning?

使用類似的思路，該模型是否也可以用於在進行預測之前產生知識？這正是[Liu et al. 2022](https://arxiv.org/pdf/2110.08387.pdf)的論文中所嘗試的——產生可用作提示的知識。特別是，這對於常識推理等任務有多大幫助？

Let's try a simple prompt:

讓我們試試一個簡單的提示：

*Prompt:*

*提示:*

```
Part of golf is trying to get a higher point total than others. Yes or No?
```

*Output:*

*輸出:*

```
Yes.
```

This type of mistake reveals the limitations of LLMs to perform tasks that require more knowledge about the world. How do we improve this with knowledge generation?

這種錯誤顯示了LLMs在執行需要更多關於世界知識的任務方面的限制。我們如何透過知識產生來改善這種情況？

First, we generate a few "knowledges":

首先，我們產生一些“知識”：

*Prompt:*

*提示:*

```
Input: Greece is larger than mexico.
Knowledge: Greece is approximately 131,957 sq km, while Mexico is approximately 1,964,375 sq km, making Mexico 1,389% larger than Greece.

Input: Glasses always fog up.
Knowledge: Condensation occurs on eyeglass lenses when water vapor from your sweat, breath, and ambient humidity lands on a cold surface, cools, and then changes into tiny drops of liquid, forming a film that you see as fog. Your lenses will be relatively cool compared to your breath, especially when the outside air is cold.

Input: A fish is capable of thinking.
Knowledge: Fish are more intelligent than they appear. In many areas, such as memory, their cognitive powers match or exceed those of ’higher’ vertebrates including non-human primates. Fish’s long-term memories help them keep track of complex social relationships.

Input: A common effect of smoking lots of cigarettes in one’s lifetime is a higher than normal chance of getting lung cancer.
Knowledge: Those who consistently averaged less than one cigarette per day over their lifetime had nine times the risk of dying from lung cancer than never smokers. Among people who smoked between one and 10 cigarettes per day, the risk of dying from lung cancer was nearly 12 times higher than that of never smokers.

Input: A rock is the same size as a pebble.
Knowledge: A pebble is a clast of rock with a particle size of 4 to 64 millimetres based on the Udden-Wentworth scale of sedimentology. Pebbles are generally considered larger than granules (2 to 4 millimetres diameter) and smaller than cobbles (64 to 256 millimetres diameter).

Input: Part of golf is trying to get a higher point total than others.
Knowledge:
```

*Knowledge 1:*

*知識 1:*

```
The objective of golf is to play a set of holes in the least number of strokes. A round of golf typically consists of 18 holes. Each hole is played once in the round on a standard golf course. Each stroke is counted as one point, and the total number of strokes is used to determine the winner of the game.
```

*Knowledge 2:*

*知識 2:*

```
Golf is a precision club-and-ball sport in which competing players (or golfers) use many types of clubs to hit balls into a series of holes on a course using the fewest number of strokes. The goal is to complete the course with the lowest score, which is calculated by adding up the total number of strokes taken on each hole. The player with the lowest score wins the game.
```

We are using the prompt provided in the paper by [Liu et al. 2022](https://arxiv.org/pdf/2110.08387.pdf).

我們正在使用[Liu等人2022年](https://arxiv.org/pdf/2110.08387.pdf)論文中提供的提示。

The next step is to integrate the knowledge and get a prediction. I reformatted the question into QA format to guide the answer format.

下一步是整合知識並進行預測。我將問題重新格式化為問答格式，以引導回答格式。

*Prompt:*

*提示:*

```
Question: Part of golf is trying to get a higher point total than others. Yes or No?

Knowledge: The objective of golf is to play a set of holes in the least number of strokes. A round of golf typically consists of 18 holes. Each hole is played once in the round on a standard golf course. Each stroke is counted as one point, and the total number of strokes is used to determine the winner of the game.

Explain and Answer:
```

*Answer 1 (confidence very high):*

*答案 1（非常有把握）：*

```
No, the objective of golf is not to get a higher point total than others. Rather, the objective is to play a set of holes in the least number of strokes. The total number of strokes is used to determine the winner of the game, not the total number of points.
```

```
Question: Part of golf is trying to get a higher point total than others. Yes or No?

Knowledge: Golf is a precision club-and-ball sport in which competing players (or golfers) use many types of clubs to hit balls into a series of holes on a course using the fewest number of strokes. The goal is to complete the course with the lowest score, which is calculated by adding up the total number of strokes taken on each hole. The player with the lowest score wins the game.

Explain and Answer:
```

*Answer 2 (confidence is a lot lower):*

*答案2（信心低得多）：*

```
Yes, part of golf is trying to get a higher point total than others. Each player tries to complete the course with the lowest score, which is calculated by adding up the total number of strokes taken on each hole. The player with the lowest score wins the game.
```

Some really interesting things happened with this example. In the first answer, the model was very confident but in the second not so much. I simplify the process for demonstration purposes but there are a few more details to consider when arriving at the final answer. Check out the paper for more.

這個例子發生了一些非常有趣的事情。在第一個答案中，模型非常自信，但在第二個答案中則不太自信。我為了示範目的簡化了過程，但在得出最終答案時還有一些細節需要考慮。請檢視論文以獲取更多資訊。

---

## Automatic Prompt Engineer (APE)

## 自動提示工程師 (APE)

![](../img/APE.png)

![](../img/APE.png)

[Zhou et al., (2022)](https://arxiv.org/abs/2211.01910) propose automatic prompt engineer (APE) a framework for automatic instruction generation and selection. The instruction generation problem is framed as natural language synthesis addressed as a black-box optimization problem using LLMs to generate and search over candidate solutions.

[Zhou等人，(2022)](https://arxiv.org/abs/2211.01910) 提出了自動提示工程師（APE），一個自動指令產生和選擇的框架。指令產生問題被構建為自然語言合成，使用LLMs作為黑盒優化問題來產生和搜尋候選解決方案。

The first step involves a large language model (as an inference model) that is given output demonstrations to generate instruction candidates for a task. These candidate solutions will guide the search procedure. The instructions are executed using a target model, and then the most appropriate instruction is selected based on computed evaluation scores.

第一步涉及使用大型語言模型（作為推理模型），該模型接收輸出示範以產生任務的指令候選項。這些候選解決方案將指導搜尋過程。使用目標模型執行指令，然後根據計算的評估分數選擇最合適的指令。

APE discovers a better zero-shot CoT prompt than the human engineered "Let's think step by step" prompt (Kojima et al., 2022).

APE 發現了一個比人工設計的“讓我們逐步思考”提示（Kojima等人，2022）更好的zero-shot CoT提示。

The prompt "Let's work this out in a step by step way to be sure we have the right answer." elicits chain-of-though reasoning and improves performance on the MultiArith and GSM8K benchmarks:

提示：“讓我們逐步解決問題，確保我們得到正確的答案。” 可以引發思維鏈式反應，並提高MultiArith和GSM8K基準測試的效能：

![](../img/ape-zero-shot-cot.png)

![](../img/ape-zero-shot-cot.png)

This paper touches on an important topic related to prompt engineering which is the idea of automatically optimizing prompts. While we don't go deep into this topic in this guide, here are a few key papers if you are interested in the topic:

本文涉及與快速工程相關的重要主題，即自動優化提示的想法。雖然我們在本指南中沒有深入探討這個主題，但如果您對此感興趣，以下是一些關鍵論文：

- [AutoPrompt](https://arxiv.org/abs/2010.15980) - proposes an approach to automatically create prompts for a diverse set of tasks based on gradient-guided search.
- [Prefix Tuning](https://arxiv.org/abs/2101.00190) - a lightweight alternative to fine-tuning that prepends a trainable continuous prefix for NLG tasks.
- [Prompt Tuning](https://arxiv.org/abs/2104.08691) - proposes a mechanism for learning soft prompts through backpropagation.


---

- [AutoPrompt](https://arxiv.org/abs/2010.15980) - 提出了一種基於梯度引導搜尋的方法，自動為各種任務建立提示。
- [Prefix Tuning](https://arxiv.org/abs/2101.00190) - 是一種輕量級的fine-tuning替代方案，為NLG任務新增了一個可訓練的連續字首。
- [Prompt Tuning](https://arxiv.org/abs/2104.08691) - 提出了一種透過反向傳播學習軟提示的機制。

---

[Previous Section (Basic Prompting)](./prompts-basic-usage.md)

[前一節（基本提示）](./prompts-basic-usage.md)

[Next Section (Applications)](./prompts-applications.md)

[下一節 (應用)](./prompts-applications.md)

