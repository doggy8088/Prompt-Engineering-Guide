# ChatGPT Prompt Engineering

# ChatGPT 提示工程

In this section, we cover the latest prompt engineering techniques for ChatGPT, including tips, applications, limitations, papers, and additional reading materials.

在本節中，我們介紹了最新的ChatGPT提示工程技術，包括技巧、應用、限制、論文和其他閱讀材料。

**Note that this section is under heavy development.**

請注意，此部分正在大力開發中。

Topics:

主題：

- [ChatGPT Prompt Engineering](#chatgpt-prompt-engineering)
- [ChatGPT 提示工程](#chatgpt-提示工程)
  - [ChatGPT Introduction](#chatgpt-introduction)
  - [ChatGPT 簡介](#chatgpt-簡介)
  - [Reviewing The Conversation Task](#reviewing-the-conversation-task)
  - [審查對話任務](#審查對話任務)
  - [Conversations with ChatGPT](#conversations-with-chatgpt)
  - [與ChatGPT的對話](#與chatgpt的對話)
    - [Multi-turn Conversations](#multi-turn-conversations)
    - [多回合對話](#多回合對話)
    - [Single-turn tasks](#single-turn-tasks)
    - [單輪任務](#單輪任務)
    - [Instructing Chat Models](#instructing-chat-models)
    - [指導聊天模型](#指導聊天模型)
  - [Python Notebooks](#python-notebooks)
  - [Python筆記本](#python筆記本)
  - [References](#references)
  - [參考文獻](#參考文獻)


---

- [ChatGPT 簡介](#chatgpt-introduction)
- [回顧對話任務](#reviewing-the-conversation-task)
- [與 ChatGPT 對話](#conversations-with-chatgpt)
- [Python 筆記本](#python-notebooks)

---

## ChatGPT Introduction

## ChatGPT 簡介

ChatGPT is a new model [trained by OpenAI](https://openai.com/blog/chatgpt) that can interact conversationally. This model is trained to follow instructions in a prompt to provide appropriate responses in the context of a dialogue. ChatGPT can help with answering questions, suggesting recipes, writing lyrics in a certain style, generating code, and much more.

ChatGPT是一個由OpenAI訓練的新模型，可以進行對話互動。該模型經過訓練，可以按照提示進行指令，以在對話上下文中提供適當的回應。ChatGPT可以幫助回答問題，建議食譜，以某種風格撰寫歌詞，產生程式碼等等。

ChatGPT is trained using Reinforcement Learning from Human Feedback (RLHF). While this model is a lot more capable than previous GPT iterations (and also trained to reduce harmful and untruthful outputs), it still comes with limitations. Let's cover some of the capabilities and limitations with concrete examples.

ChatGPT是使用人類反饋的強化學習（RLHF）進行訓練的。雖然這個模型比以前的GPT迭代模型更加強大（並且還經過訓練以減少有害和不真實的輸出），但它仍然有一些限制。讓我們透過具體的例子來介紹一些能力和限制。

You can use the research preview of ChatGPT [here](https://chat.openai.com) but for the examples below we will use the `Chat` mode on the OpenAI Playground.

你可以在此使用 ChatGPT 的研究預覽 [連結](https://chat.openai.com)，但在下面的示範中，我們將使用 OpenAI Playground 上的“Chat”模式。

---

## Reviewing The Conversation Task

## 審查對話任務

In one of the [previous guides](https://github.com/dair-ai/Prompt-Engineering-Guide/blob/main/guides/prompts-basic-usage.md#conversation), we covered a bit about conversation capabilities and role prompting. We covered how to instruct the LLM to have a conversation in a specific style, with a specific intent, behavior, and identity.

在[之前的指南](https://github.com/dair-ai/Prompt-Engineering-Guide/blob/main/guides/prompts-basic-usage.md#conversation)中，我們介紹了一些關於對話能力和角色提示的內容。我們介紹了如何指示LLM以特定的風格、特定的意圖、行為和身份進行對話。

Let's review our previous basic example where we created a conversational system that's able to generate more technical and scientific responses to questions.

讓我們回顧一下我們之前的基本範例，我們建立了一個對話系統，能夠對問題產生更多技術和科學方面的回答。

*Prompt:*

*提示:*

```
The following is a conversation with an AI research assistant. The assistant tone is technical and scientific.

Human: Hello, who are you?
AI: Greeting! I am an AI research assistant. How can I help you today?
Human: Can you tell me about the creation of black holes?
AI:
```

From the example above, you can see two important components:

從上面的例子中，您可以看到兩個重要的元件：

- the **intent** or explanation of what the chatbot is
- the **identity** which instructs the style or tone the chatbot will use to respond


---

- 聊天機器人的**意圖**或解釋
- 指示聊天機器人回應時使用的風格或語調的**身份**

The simple example above works well with the text completion APIs that use `text-davinci-003`. More recently, OpenAI [announced the ChatGPT APIs](https://openai.com/blog/introducing-chatgpt-and-whisper-apis), which is a more powerful and cheaper model called `gpt-3.5-turbo` was specifically built for this type of functionality (chat completions). OpenAI recommends this as their best model even for non-chat use cases.  Other benefits of using the ChatGPT APIs are significant cost reduction (90%) and efficiency.

以上簡單的範例適用於使用 `text-davinci-003` 的文字完成 API。最近，OpenAI [宣佈了 ChatGPT APIs](https://openai.com/blog/introducing-chatgpt-and-whisper-apis)，這是一個更強大且更便宜的模型，名為 `gpt-3.5-turbo`，專門為此類功能（聊天完成）而建立。OpenAI 建議即使在非聊天使用情況下，也使用此模型作為最佳模型。使用 ChatGPT APIs 的其他好處是顯著的成本降低（90％）和效率。

Big companies like Snap Inc. and Instacart are already integrating conversational features powered by ChatGPT on their products that range from personalized recommendations to open-ended shopping goals.

像Snap Inc.和Instacart這樣的大公司已經在他們的產品中整合了由ChatGPT提供支援的對話功能，這些產品從個性化推薦到開放式購物目標不等。

---

## Conversations with ChatGPT

## 與ChatGPT的對話

### Multi-turn Conversations

### 多回合對話

To begin demonstrating the capabilities of ChatGPT, we will use the chatbot assistant example above and discuss the results. Compared to `text-davinci-003`, the `gpt-3.5-turbo` model that powers ChatGPT uses a chat format as input. The model expects a series of messages as input and uses those to generate a response.

開始展示 ChatGPT 的能力，我們將使用上面的聊天機器人助手示範並討論結果。與 `text-davinci-003` 相比，驅動 ChatGPT 的 `gpt-3.5-turbo` 模型使用聊天格式作為輸入。該模型期望一系列訊息作為輸入，並使用這些訊息產生回復。

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

請注意，上面的示範中，我簡化了輸入和輸出，但 ChatGPT 聊天完成 API 要求訊息以特定格式呈現。我在下面新增了一個快照，展示了在 OpenAI Playground 中使用 `聊天模式` 的示範。

![](../img/chatgpt-1.png)

![](../img/chatgpt-1.png)

The more formal API call for our example would look something like the example below:

我們的範例中，更正式的 API 呼叫看起來會像下面的例子：

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

The way developers interact with ChatGPT in the future is expected to be done via the [Chat Markup Language](https://github.com/openai/openai-python/blob/main/chatml.md) (ChatML for short).

未來開發人員與ChatGPT的互動預計將透過[Chat Markup Language](https://github.com/openai/openai-python/blob/main/chatml.md)（簡稱ChatML）完成。

### Single-turn tasks

### 單輪任務

The chat format enables multi-turn conversations but it also supports single-turn tasks similar to what we used with `text-davinci-003`. This means we can use ChatGPT to perform similar tasks as what we have demonstrated for the original GPT models. For example, let's try to perform the following question-answering task using ChatGPT:

聊天格式支援多回合對話，但也支援類似於我們使用的 `text-davinci-003` 的單回合任務。這意味著我們可以使用 ChatGPT 執行與我們為原始 GPT 模型示範的類似任務。例如，讓我們嘗試使用 ChatGPT 執行以下問答任務：

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

更正式地說，這是 API 呼叫（我只包含了請求的訊息元件）。

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

### Instructing Chat Models

### 指導聊天模型

According to the official OpenAI docs, snapshots of the `gpt-3.5-turbo` model will also be made available. For example, we can access the snapshot from March 1 `gpt-3.5-turbo-0301`. This allows developers to opt for specific model versions. This also means that the best practices for instructing models may change from version to version.

根據官方的OpenAI文件，`gpt-3.5-turbo`模型的快照也將可用。例如，我們可以訪問3月1日的快照`gpt-3.5-turbo-0301`。這使得開發人員可以選擇特定的模型版本。這也意味著指導模型的最佳實踐可能會因版本而異。

The current recommendation for `gpt-3.5-turbo-0301` is to add instructions in the `user` message as opposed to the available `system` message.

目前對於 `gpt-3.5-turbo-0301` 的建議是在 `user` 訊息中新增指示，而不是使用可用的 `system` 訊息。

---

## Python Notebooks

## Python筆記本

|Description|Notebook|
|--|--|
|Learn more about how to make calls to the ChatGPT APIs using the `openai` library.|[ChatGPT API Intro](../notebooks/pe-chatgpt-intro.ipynb)|
|Learn how to use ChatGPT features using the `LangChain` library. |[ChatGPT API with LangChain](../notebooks/pe-chatgpt-langchain.ipynb)|

|描述|筆記本|
|--|--|
|使用 `openai` 函式庫更深入地瞭解如何呼叫 ChatGPT API。|[ChatGPT API 簡介](../notebooks/pe-chatgpt-intro.ipynb)|
|使用 `LangChain` 函式庫學習如何使用 ChatGPT 功能。|[ChatGPT API with LangChain](../notebooks/pe-chatgpt-langchain.ipynb)|

---

## References

## 參考文獻

- [Seeing ChatGPT Through Students' Eyes: An Analysis of TikTok Data](https://arxiv.org/abs/2303.05349) (March 2023)
- [Extracting Accurate Materials Data from Research Papers with Conversational Language Models and Prompt Engineering -- Example of ChatGPT](https://arxiv.org/abs/2303.05352) (Mar 2023)
- [ChatGPT is on the horizon: Could a large language model be all we need for Intelligent Transportation?](https://arxiv.org/abs/2303.05382) (Mar 2023)
- [Making a Computational Attorney](https://arxiv.org/abs/2303.05383) (Mar 2023)
- [Does Synthetic Data Generation of LLMs Help Clinical Text Mining?](https://arxiv.org/abs/2303.04360) (Mar 2023)
- [MenuCraft: Interactive Menu System Design with Large Language Models](https://arxiv.org/abs/2303.04496) (Mar 2023)
- [A Comprehensive Survey of AI-Generated Content (AIGC): A History of Generative AI from GAN to ChatGPT](https://arxiv.org/abs/2303.04226) (Mar 2023)
- [Exploring the Feasibility of ChatGPT for Event Extraction](https://arxiv.org/abs/2303.03836)
- [ChatGPT: Beginning of an End of Manual Annotation? Use Case of Automatic Genre Identification](https://arxiv.org/abs/2303.03953) (Mar 2023)
- [Is ChatGPT a Good NLG Evaluator? A Preliminary Study](https://arxiv.org/abs/2303.04048) (Mar 2023)
- [Will Affective Computing Emerge from Foundation Models and General AI? A First Evaluation on ChatGPT](https://arxiv.org/abs/2303.03186) (Mar 2023)
- [UZH_CLyp at SemEval-2023 Task 9: Head-First Fine-Tuning and ChatGPT Data Generation for Cross-Lingual Learning in Tweet Intimacy Prediction](https://arxiv.org/abs/2303.01194) (Mar 2023)
- [How to format inputs to ChatGPT models](https://github.com/openai/openai-cookbook/blob/main/examples/How_to_format_inputs_to_ChatGPT_models.ipynb) (Mar 2023)
- [Can ChatGPT Assess Human Personalities? A General Evaluation Framework](https://arxiv.org/abs/2303.01248) (Mar 2023)
- [Cross-Lingual Summarization via ChatGPT](https://arxiv.org/abs/2302.14229) (Feb 2023)
- [ChatAug: Leveraging ChatGPT for Text Data Augmentation](https://arxiv.org/abs/2302.13007) (Feb 2023)
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
- [Exploring AI Ethics of ChatGPT: A Diagnostic Analysis](https://arxiv.org/abs/2301.12867) (Jan 2023)
- [ChatGPT for Good? On Opportunities and Challenges of Large Language Models for Education](https://www.edu.sot.tum.de/fileadmin/w00bed/hctl/_my_direct_uploads/ChatGPT_for_Good_.pdf) (Jan 2023)
- [The political ideology of conversational AI: Converging evidence on ChatGPT's pro-environmental, left-libertarian orientation](https://arxiv.org/abs/2301.01768) (Jan 2023)
- [Techniques to improve reliability - OpenAI Cookbook](https://github.com/openai/openai-cookbook/blob/main/techniques_to_improve_reliability.md)
- [Awesome ChatGPT Prompts](https://github.com/f/awesome-chatgpt-prompts)
- [Introducing ChatGPT](https://openai.com/blog/chatgpt) (Nov 2022)


---

- [透過學生的眼睛看ChatGPT：TikTok資料分析](https://arxiv.org/abs/2303.05349) (2023年3月)
- [使用對話式語言模型和提示工程從研究論文中提取準確的材料資料--以ChatGPT為例](https://arxiv.org/abs/2303.05352) (2023年3月)
- [ChatGPT在地平線上：一個大型語言模型是否足以滿足智慧交通的需求？](https://arxiv.org/abs/2303.05382) (2023年3月)
- [打造計算律師](https://arxiv.org/abs/2303.05383) (2023年3月)
- [LLMs的合成資料產生是否有助於臨床文字挖掘？](https://arxiv.org/abs/2303.04360) (2023年3月)
- [使用大型語言模型進行互動式菜單系統設計](https://arxiv.org/abs/2303.04496) (2023年3月)
- [AI產生內容（AIGC）的全面調查：從GAN到ChatGPT的產生AI歷史](https://arxiv.org/abs/2303.04226) (2023年3月)
- [探索ChatGPT在事件提取中的可行性](https://arxiv.org/abs/2303.03836)
- [ChatGPT：手動標註的終結？自動體裁識別的應用案例](https://arxiv.org/abs/2303.03953) (2023年3月)
- [ChatGPT是否是一個好的NLG評估器？初步研究](https://arxiv.org/abs/2303.04048) (2023年3月)
- [情感計算是否會從基礎模型和通用AI中出現？對ChatGPT的初步評估](https://arxiv.org/abs/2303.03186) (2023年3月)
- [UZH_CLyp在SemEval-2023任務9中的表現：頭部優先微調和ChatGPT資料產生用於跨語言學習的Tweet親密度預測](https://arxiv.org/abs/2303.01194) (2023年3月)
- [如何為ChatGPT模型格式化輸入](https://github.com/openai/openai-cookbook/blob/main/examples/How_to_format_inputs_to_ChatGPT_models.ipynb) (2023年3月)
- [ChatGPT能評估人類個性嗎？一個通用的評估框架](https://arxiv.org/abs/2303.01248) (2023年3月)
- [透過ChatGPT進行跨語言總結](https://arxiv.org/abs/2302.14229) (2023年2月)
- [ChatAug：利用ChatGPT進行文字資料擴充](https://arxiv.org/abs/2302.13007) (2023年2月)
- [Dr ChatGPT，告訴我我想聽的：提示知識如何影響健康答案的正確性](https://arxiv.org/abs/2302.13793) (2023年2月)
- [對數學單詞問題（MWP）的ChatGPT進行獨立評估](https://arxiv.org/abs/2302.13814) (2023年2月)
- [ChatGPT：2.5個月後的元分析](https://arxiv.org/abs/2302.13795) (2023年2月)
- [讓我們聊聊！與ChatGPT的對話：技術，應用和限制](https://arxiv.org/abs/2302.13817) (2023年2月)
- [檢查您的事實並重試：使用外部知識和自動反饋改進大型語言模型](https://arxiv.org/abs/2302.12813) (2023年2月)
- [關於ChatGPT的魯棒性：對抗和分佈外的觀點](https://arxiv.org/abs/2302.12095) (2023年2月)
- [ChatGPT等產生AI模型在SPC實踐，教育和研究中如何（誤）用？一項探索性研究](https://arxiv.org/abs/2302.10916) (2023年2月)
- [ChatGPT和Fine-tuned BERT的比較研究](https://arxiv.org/abs/2302.10198) (2023年2月)
- [一個提示模式目錄，以增強使用ChatGPT的提示工程](https://arxiv.org/abs/2302.11382) (2023年2月)
- [透過與ChatGPT聊天實現zero-shot資訊擷取](https://arxiv.org/abs/2302.10205) (2023年2月)
- [ChatGPT：萬能但不精通](https://arxiv.org/abs/2302.10724) (2023年2月)
- [ChatGPT和DALL-E 2在決策和空間推理方面的初步評估](https://arxiv.org/abs/2302.09068) (2023年2月)
- [網民，學者和資訊專業人員對AI的看法，特別是ChatGPT](https://arxiv.org/abs/2302.07136) (2023年2月)
- [ChatGPT中的語言歧義分析](https://arxiv.org/abs/2302.06426) (2023年2月)
- [ChatGPT與傳統問答系統在知識圖譜上的比較：當前狀態和未來方向向知識圖譜聊天機器人](https://arxiv.org/abs/2302.06466) (2023年2月)
- [ChatGPT和產生AI對科學意味著什麼](https://www.nature.com/articles/d41586-023-00340-6) (2023年2月)
- [應用BERT和ChatGPT對科學文獻中的萊姆病進行情感分析](https://arxiv.org/abs/2302.06474) (2023年2月)
- [探索ChatGPT的AI倫理：一項診斷性分析](https://arxiv.org/abs/2301.12867) (2023年1月)
- [ChatGPT為好？關於大型語言模型在教育中的機會和挑戰](https://www.edu.sot.tum.de/fileadmin/w00bed/hctl/_my_direct_uploads/ChatGPT_for_Good_.pdf) (2023年1月)
- [對話式AI的政治意識形態：ChatGPT的環保主義，左翼自由主義取向的證據匯總](https://arxiv.org/abs/2301.01768) (2023年1月)
- [提高可靠性的技術-OpenAI Cookbook](https://github.com/openai/openai-cookbook/blob/main/techniques_to_improve_reliability.md)
- [Awesome ChatGPT Prompts](https://github.com/f/awesome-chatgpt-prompts)
- [介紹ChatGPT](https://openai.com/blog/chatgpt) (2022年11月)

---

[Previous Section (Applications)](./prompts-applications.md)

[上一節（應用）](./prompts-applications.md)

[Next Section (Adversarial Prompting)](./prompts-adversarial.md)

[下一節（對抗性提示）](./prompts-adversarial.md)

