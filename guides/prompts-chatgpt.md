# ChatGPT Prompt Engineering

# ChatGPT提示工程

In this section, we cover the latest prompt engineering techniques for ChatGPT, including tips, applications, limitations, papers, and additional reading materials.

在本節中，我們介紹了最新的 ChatGPT 提示工程技術，包括技巧、應用、限制、論文和其他閱讀材料。

**Note that this section is under heavy development.**

請注意，此部分正在大力開發中。

Topics:

主題：

- [ChatGPT Introduction](#chatgpt-introduction)
- [Reviewing The Conversation Task](#reviewing-the-conversation-task)
- [Converstions with ChatGPT](#conversations-with-chatgpt)


---

- [ChatGPT 簡介](#chatgpt-introduction)
- [審查對話任務](#reviewing-the-conversation-task)
- [與 ChatGPT 對話](#conversations-with-chatgpt)

---

## ChatGPT Introduction

## ChatGPT 簡介

ChatGPT is a new model [trained by OpenAI](https://openai.com/blog/chatgpt) that has the capability to interact in a conversational way. This model is trained to follow instructions in a prompt to provide appropriate responses in the context of a dialogue. ChatGPT can help with answering questions, suggest recipes, write lyrics in a certain style, generate code, and much more.

ChatGPT 是一個由 OpenAI 訓練的新模型，具有互動式對話的能力。該模型經過訓練，可以按照提示執行指令，以在對話的上下文中提供適當的回應。ChatGPT 可以幫助回答問題、建議食譜、以特定風格編寫歌詞、產生程式碼等等。

ChatGPT is trained using Reinforcement Learning from Human Feedback (RLHF). While this model is a lot more capable than previous GPT iterations (and also trained to reduce harmful and untruthful outputs), it still comes with limitations. Let's cover some of the capabilities and limitations with concrete examples. 

ChatGPT是使用人類反饋的強化學習（RLHF）進行訓練的。雖然該模型比以前的GPT迭代更具備能力（並且還經過訓練以減少有害和不真實的輸出），但它仍然存在一些限制。讓我們透過具體的例子來介紹一些能力和限制。

You can use the research preview of ChatGPT [here](chat.openai.com) but for the examples we will use the Chat mode on the OpenAI Playground.

你可以在這裡使用 ChatGPT 的研究預覽 [chat.openai.com]，但是我們的示範將使用 OpenAI Playground 上的 Chat 模式。

---

## Reviewing The Conversation Task

## 審查對話任務

In one of the previous guides, we covered a bit about conversation capabilities and role prompting. We covered how to instruct the LLM to have a conversation in a specific style, with a specific intent, behavior, and identity.

在之前的指南中，我們談到了關於對話能力和角色提示的一些內容。我們介紹了如何指示LLM以特定風格、特定意圖、行為和身份進行對話。

Let's review our previous basic example where we created a conversational system that's able to generate more technical and scientific responses to questions. 

讓我們回顧一下之前的基本範例，我們建立了一個對話系統，能夠對問題產生更多技術性和科學性的回答。

*Prompt:*

*提示：*

```
The following is a conversation with an AI research assistant. The assistant tone is technical and scientific.

Human: Hello, who are you?
AI: Greeting! I am an AI research assistant. How can I help you today?
Human: Can you tell me about the creation of blackholes?
AI:
```

From the example above, you can see two important components:

從上面的例子可以看出兩個重要的組成部分：

- the **intent** or explanation of what the chatbot is
- the **identity** which instructs the style or tone the chatbot will use to respond


---

- 聊天機器人的**意圖**或解釋
- 聊天機器人將用來回應的**身份**或風格

The simple example above works well with the text completion APIs that uses `text-davinci-003`. More recently, OpenAI [announced the ChatGPT APIs](https://openai.com/blog/introducing-chatgpt-and-whisper-apis), which is a more powerful and cheaper model called `gpt-3.5-turbo` was specifically built for this type of functionality (chat completions). In fact, OpenAI recommends this is as their best model even for non-chat use cases.  Other benefits of using the ChatGPT APIs are the significant cost reduction (90%) and efficiency. 

以上簡單的例子適用於使用 `text-davinci-003` 的文字完成 API。最近，OpenAI [宣佈了 ChatGPT API](https://openai.com/blog/introducing-chatgpt-and-whisper-apis)，這是一個更強大和更便宜的模型，名為 `gpt-3.5-turbo`，專門為這種功能（聊天完成）而建立。事實上，OpenAI 建議即使對於非聊天用例，也應使用這是他們最好的模型。使用 ChatGPT API 的其他好處是顯著降低成本（90％）和提高效率。

Big companies like Snap Inc. and Instacart are already integrating conversational features powered by ChatGPT on their products that range from personalized recommendations to open-ended shopping goals.

像 Snap Inc. 和 Instacart 這樣的大公司已經在他們的產品中整合了由 ChatGPT 提供動力的對話功能，這些產品從個性化推薦到開放式購物目標不等。

---

## Conversations with ChatGPT

## 與ChatGPT的對話

To begin demonstrating the capabilities of ChatGPT, we will use the chatbot assistant example above and discuss results. Compared to `text-davinci-003`, the `gpt-3.5-turbo` model that powers ChatGPT uses a chat format as input. The model expects a series of messages as input, and uses those to generate a response. 

展示ChatGPT的能力，我們將使用上述聊天機器人助手的例子並討論結果。相比`text-davinci-003`，驅動ChatGPT的 `gpt-3.5-turbo` 模型使用聊天格式作為輸入。該模型期望一系列訊息作為輸入並使用它們產生回復。

*Input:*

*輸入:*

```
SYSTEM: You are an AI research assistant. You use a tone that is technical and scientific.
USER: Hello, who are you?
ASSISTANT: Greeting! I am an AI research assistant. How can I help you today?
USER: Can you tell me about the creation of black holes?
```

*Output:*

*輸出:*

```
ASSISTANT: Of course! Black holes are created when a massive star runs out of fuel and collapses under its own gravity. This collapse causes the star to become incredibly dense, with a gravitational field so strong that not even light can escape it. This point of no return is known as the event horizon. Once a star crosses the event horizon, it is considered a black hole...
```

Note that in the example above, I have simplified the input and output but the ChatGPT chat completion API requires messages to be in a specific format. I have added a snapshot below of how this example would look using the `Chat Mode` in the OpenAI Playground:

請注意，上述示範中，我簡化了輸入和輸出，但 ChatGPT 聊天完成 API 需要訊息以特定格式提供。我在下面新增了一個快照，展示了在 OpenAI Playground 中使用 `Chat Mode` 的這個示範的樣子：

![](../img/chatgpt-1.png)

![](../img/chatgpt-1.png)

The more formal API call for our example would look something like the example below:

我們的範例所需的較正式的 API 呼叫看起來會像以下的範例：

```python
import openai

openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
        {"role": "system", "content": "You are an AI research assistant. You use a tone that is technical and scientific."},
        {"role": "user", "content": "Hello, who are you?"},
        {"role": "assistant", "content": "Greeting! I am an AI research assistant. How can I help you today?"},
        {"role": "user", "content": "Can you tell me about the creation of black holes?"}
    ]
)
```

In fact, the way developers interact with ChatGPT in the future is expected to be done via the [Chat Markup Language](https://github.com/openai/openai-python/blob/main/chatml.md) (ChatML for short).

事實上，未來開發者與ChatGPT互動的方式預計將透過[聊天標記語言](https://github.com/openai/openai-python/blob/main/chatml.md)（簡稱ChatML）完成。

The chat format enables multi-turn conversations but it also supports single-turn tasks similar to what we used with `text-davinci-003`. This means we can use ChatGPT to perform similar tasks as what we have demonstrated for the original GPT models. For example, let's try to perform the following question answering task using ChatGPT:

聊天格式可以進行多輪對話，但它也支援類似於我們使用`text-davinci-003`的單輪任務。這意味著我們可以使用ChatGPT執行與我們為原始GPT模型示範的類似任務。例如，讓我們嘗試使用ChatGPT執行以下問答任務：

*Input:*

*輸入:*

```
USER: Answer the question based on the context below. Keep the answer short and concise. Respond "Unsure about answer" if not sure about the answer.

Context: Teplizumab traces its roots to a New Jersey drug company called Ortho Pharmaceutical. There, scientists generated an early version of the antibody, dubbed OKT3. Originally sourced from mice, the molecule was able to bind to the surface of T cells and limit their cell-killing potential. In 1986, it was approved to help prevent organ rejection after kidney transplants, making it the first therapeutic antibody allowed for human use.

Question: What was OKT3 originally sourced from?

Answer:
```

*Output:*

*輸出:*

```
ASSISTANT: Mice.
```

Keep in mind that I am adding the `USER` and `ASSISTANT` labels to better demonstrate how the task can be performed using ChatGPT. Here is the example using the Playground:

請記住，我正在新增“USER”和“ASSISTANT”標籤，以更好地示範如何使用ChatGPT執行任務。以下是使用Playground的示範：

![](../img/chatgpt-classic.png)

![](../img/chatgpt-classic.png)

More formally, this is the API call (I've only included the message component of the request):

更正式地說，這是 API 呼叫（我只包含了請求的訊息元件）：

```python
CONTENT = """Answer the question based on the context below. Keep the answer short and concise. Respond \"Unsure about answer\" if not sure about the answer.

Context: Teplizumab traces its roots to a New Jersey drug company called Ortho Pharmaceutical. There, scientists generated an early version of the antibody, dubbed OKT3. Originally sourced from mice, the molecule was able to bind to the surface of T cells and limit their cell-killing potential. In 1986, it was approved to help prevent organ rejection after kidney transplants, making it the first therapeutic antibody allowed for human use.

Question: What was OKT3 originally sourced from?

Answer:
"""

response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=[
        {"role": "user", "content": CONTENT},
    ],
    temperature=0,
)
```

You can try out [this notebook](../notebooks/pe-chatgpt-intro.ipynb) to learn more about how to make calls to the ChatGPT APIs. 

你可以嘗試使用[這個筆記本](../notebooks/pe-chatgpt-intro.ipynb)來瞭解如何呼叫ChatGPT API。

---

More coming soon!

More coming soon!

---

## References

## 參考文獻

- [UZH_CLyp at SemEval-2023 Task 9: Head-First Fine-Tuning and ChatGPT Data Generation for Cross-Lingual Learning in Tweet Intimacy Prediction](https://arxiv.org/abs/2303.01194) (Mar 2023)
- [How to format inputs to ChatGPT models](https://github.com/openai/openai-cookbook/blob/main/examples/How_to_format_inputs_to_ChatGPT_models.ipynb) (Mar 2023)
- [Can ChatGPT Assess Human Personalities? A General Evaluation Framework](https://arxiv.org/abs/2303.01248) (Mar 2023)
- [Cross-Lingual Summarization via ChatGPT](https://arxiv.org/abs/2302.14229) (Feb 2023)
- [Dr ChatGPT, tell me what I want to hear: How prompt knowledge impacts health answer correctness](https://arxiv.org/abs/2302.13793) (Feb 2023)
- [An Independent Evaluation of ChatGPT on Mathematical Word Problems (MWP)](https://arxiv.org/abs/2302.13814) (Feb 2023)
- [ChatGPT: A Meta-Analysis after 2.5 Months](https://arxiv.org/abs/2302.13795) (Feb 2023)
- [Let's have a chat! A Conversation with ChatGPT: Technology, Applications, and Limitations](https://arxiv.org/abs/2302.13817) (Feb 2023)
- [Check Your Facts and Try Again: Improving Large Language Models with External Knowledge and Automated Feedback](https://arxiv.org/abs/2302.12813) (Feb 2023)
- [On the Robustness of ChatGPT: An Adversarial and Out-of-distribution Perspective](https://arxiv.org/abs/2302.12095) (Feb 2023)
- [How Generative AI models such as ChatGPT can be (Mis)Used in SPC Practice, Education, and Research? An Exploratory Study](https://arxiv.org/abs/2302.10916) (Feb 2023)
- [Can ChatGPT Understand Too? A Comparative Study on ChatGPT and Fine-tuned BERT](https://arxiv.org/abs/2302.10198) (Feb 2023)
- [A Prompt Pattern Catalog to Enhance Prompt Engineering with ChatGPT](https://arxiv.org/abs/2302.11382) (Feb 2023)
- [Zero-Shot Information Extraction via Chatting with ChatGPT](https://arxiv.org/abs/2302.10205) (Feb 2023)
- [ChatGPT: Jack of all trades, master of none](https://arxiv.org/abs/2302.10724) (Feb 2023)
- [A Pilot Evaluation of ChatGPT and DALL-E 2 on Decision Making and Spatial Reasoning](https://arxiv.org/abs/2302.09068) (Feb 2023)
- [Netizens, Academicians, and Information Professionals' Opinions About AI With Special Reference To ChatGPT](https://arxiv.org/abs/2302.07136) (Feb 2023)
- [Linguistic ambiguity analysis in ChatGPT](https://arxiv.org/abs/2302.06426) (Feb 2023)
- [ChatGPT versus Traditional Question Answering for Knowledge Graphs: Current Status and Future Directions Towards Knowledge Graph Chatbots](https://arxiv.org/abs/2302.06466) (Feb 2023)
- [What ChatGPT and generative AI mean for science](https://www.nature.com/articles/d41586-023-00340-6) (Feb 2023)
- [Applying BERT and ChatGPT for Sentiment Analysis of Lyme Disease in Scientific Literature](https://arxiv.org/abs/2302.06474) (Feb 2023)
- [ChatGPT for Good? On Opportunities and Challenges of Large Language Models for Education](https://www.edu.sot.tum.de/fileadmin/w00bed/hctl/_my_direct_uploads/ChatGPT_for_Good_.pdf) (Jan 2023)
- [Techniques to improve reliability - OpenAI Cookbook](https://github.com/openai/openai-cookbook/blob/main/techniques_to_improve_reliability.md)
- [Awesome ChatGPT Prompts](https://github.com/f/awesome-chatgpt-prompts)
- [Introducing ChatGPT](https://openai.com/blog/chatgpt) (Nov 2022)


---

- [UZH_CLyp at SemEval-2023 Task 9: Head-First Fine-Tuning and ChatGPT Data Generation for Cross-Lingual Learning in Tweet Intimacy Prediction](https://arxiv.org/abs/2303.01194) (2023年3月)
- [如何格式化ChatGPT模型的輸入](https://github.com/openai/openai-cookbook/blob/main/examples/How_to_format_inputs_to_ChatGPT_models.ipynb) (2023年3月)
- [ChatGPT能否評估人類個性？一般性評估框架](https://arxiv.org/abs/2303.01248) (2023年3月)
- [透過ChatGPT進行跨語言摘要](https://arxiv.org/abs/2302.14229) (2023年2月)
- [ChatGPT醫生，請告訴我我想聽的：提示知識如何影響健康答案的正確性](https://arxiv.org/abs/2302.13793) (2023年2月)
- [ChatGPT在數學單詞問題（MWP）上的獨立評估](https://arxiv.org/abs/2302.13814) (2023年2月)
- [ChatGPT：2.5個月後的元分析](https://arxiv.org/abs/2302.13795) (2023年2月)
- [讓我們聊聊！與ChatGPT的對話：技術、應用和限制](https://arxiv.org/abs/2302.13817) (2023年2月)
- [檢查你的事實並重試：透過外部知識和自動反饋來改進大型語言模型](https://arxiv.org/abs/2302.12813) (2023年2月)
- [ChatGPT的魯棒性：對抗和分佈外視角](https://arxiv.org/abs/2302.12095) (2023年2月)
- [ChatGPT等產生AI模型在SPC實踐、教育和研究中如何被（誤）用？一項探索性研究](https://arxiv.org/abs/2302.10916) (2023年2月)
- [ChatGPT能否理解？ChatGPT和Fine-tuned BERT的比較研究](https://arxiv.org/abs/2302.10198) (2023年2月)
- [增強ChatGPT提示工程的提示模式目錄](https://arxiv.org/abs/2302.11382) (2023年2月)
- [透過與ChatGPT聊天進行零-shot信息提取](https://arxiv.org/abs/2302.10205) (2023年2月)
- [ChatGPT：博而不精](https://arxiv.org/abs/2302.10724) (2023年2月)
- [ChatGPT和DALL-E 2在決策制定和空間推理方面的試點評估](https://arxiv.org/abs/2302.09068) (2023年2月)
- [網民、學者和資訊專業人員對AI的看法，特別是ChatGPT](https://arxiv.org/abs/2302.07136) (2023年2月)
- [ChatGPT中的語言歧義分析](https://arxiv.org/abs/2302.06426) (2023年2月)
- [ChatGPT與傳統問答相比，對於知識圖譜的知識圖譜聊天機器人的現狀和未來方向](https://arxiv.org/abs/2302.06466) (2023年2月)
- [ChatGPT和產生AI對科學意味著什麼](https://www.nature.com/articles/d41586-023-00340-6) (2023年2月)
- [應用BERT和ChatGPT對科學文獻中的萊姆病進行情感分析](https://arxiv.org/abs/2302.06474) (2023年2月)
- [ChatGPT的機會和挑戰：大型語言模型在教育中的應用](https://www.edu.sot.tum.de/fileadmin/w00bed/hctl/_my_direct_uploads/ChatGPT_for_Good_.pdf) (2023年1月)
- [提高可靠性的技術-OpenAI Cookbook](https://github.com/openai/openai-cookbook/blob/main/techniques_to_improve_reliability.md)
- [Awesome ChatGPT Prompts](https://github.com/f/awesome-chatgpt-prompts)
- [介紹ChatGPT](https://openai.com/blog/chatgpt) (2022年11月)

---

[Previous Section (Applications)](./prompts-applications.md)

[前一節 (應用)](./prompts-applications.md)

[Next Section (Adversarial Prompting)](./prompts-adversarial.md)

[下一節（對抗性提示）](./prompts-adversarial.md)

