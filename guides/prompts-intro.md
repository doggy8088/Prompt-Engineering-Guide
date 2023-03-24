# Prompting Introduction

# 提示性介紹

Prompt engineering is a relatively new discipline for developing and optimizing prompts to efficiently use language models (LMs) for a wide variety of applications and research topics. Prompt engineering skills help to better understand the capabilities and limitations of large language models (LLMs). Researchers use prompt engineering to improve the capacity of LLMs on a wide range of common and complex tasks such as question answering and arithmetic reasoning. Developers use prompt engineering to design robust and effective prompting techniques that interface with LLMs and other tools.

提示工程是一種相對較新的學科，用於開發和優化提示，以便有效地使用語言模型（LM）進行各種應用和研究主題。提示工程技能有助於更好地理解大型語言模型（LLM）的能力和局限性。研究人員使用提示工程來提高LLM在各種常見和複雜任務（如問答和算術推理）上的能力。開發人員使用提示工程來設計與LLM和其他工具介面的強大有效的提示技術。

This guide covers the basics of standard prompts to provide a rough idea of how to use prompts to interact and instruct large language models (LLMs).

本指南涵蓋了標準提示的基礎知識，以提供如何使用提示與大型語言模型（LLM）進行互動和指導的大致想法。

All examples are tested with `text-davinci-003` (using OpenAI's playground) unless otherwise specified. It uses the default configurations, e.g., `temperature=0.7` and `top-p=1`.

所有的例子都是使用 `text-davinci-003` 進行測試的（使用 OpenAI 的 playground），除非另有說明。它使用預設配置，例如 `temperature=0.7` 和 `top-p=1`。

Topic:

Topic: 主題

- [Basic Prompts](#basic-prompts)
- [A Word on LLM Settings](#a-word-on-llm-settings)
- [Standard Prompts](#standard-prompts)
- [Prompt Elements](#elements-of-a-prompt)
- [General Tips for Designing Prompts](#general-tips-for-designing-prompts)


---

- [基本提示](#basic-prompts)
- [關於LLM設定的說明](#a-word-on-llm-settings)
- [標準提示](#standard-prompts)
- [提示元素](#elements-of-a-prompt)
- [設計提示的一般提示](#general-tips-for-designing-prompts)

---

## Basic Prompts

## 基本提示

You can already achieve a lot with prompts, but the quality of results depends on how much information you provide it. A prompt can contain information like the `instruction` or `question` you are passing to the model and include other details such as `inputs` or `examples`.

你已經可以透過提示實現很多功能，但結果的品質取決於您提供的資訊量。提示可以包含像傳遞給模型的“指令”或“問題”之類的資訊，並包括其他詳細資訊，例如“輸入”或“示範”。

Here is a basic example of a simple prompt:

這裡是一個簡單提示的基本範例：

*Prompt*

*提示*

```
The sky is
```

*Output:*

*輸出:*

```
blue

The sky is blue on a clear day. On a cloudy day, the sky may be gray or white.
```

As you can see, the language model outputs a continuation of strings that make sense given the context `"The sky is"`. The output might be unexpected or far from the task we want to accomplish.

如您所見，語言模型輸出了一系列在給定上下文“天空是”下有意義的字串。輸出可能出乎意料或與我們想要完成的任務相距甚遠。

This basic example also highlights the necessity to provide more context or instructions on what specifically we want to achieve.

這個基本範例也凸顯了提供更多上下文或指示，以確切瞭解我們想要實現什麼的必要性。

Let's try to improve it a bit:

讓我們試著稍微改進它：

*Prompt:*

*提示:*

```
Complete the sentence:

The sky is
```

*Output:*

*輸出:*

```
 so  beautiful today.
```

Is that better? Well, we told the model to complete the sentence so the result looks a lot better as it follows exactly what we told it to do ("complete the sentence"). This approach of designing optimal prompts to instruct the model to perform a task is what's referred to as **prompt engineering**.

那樣好一些嗎？我們告訴模型完成句子，所以結果看起來更好，因為它完全按照我們告訴它要做的事情（“完成句子”）進行。設計最佳提示以指示模型執行任務的方法被稱為**提示工程**。

The example above is a basic illustration of what's possible with LLMs today. Today's LLMs can perform all kinds of advanced tasks that range from text summarization to mathematical reasoning to code generation.

以上示範是LLMs今天所能實現的基本說明。今天的LLMs可以執行各種進階任務，從文字摘要到數學推理到程式碼生成。

---

## A Word on LLM Settings

## 關於 LLM 設定

When working with prompts, you will be interacting with the LLM via an API or directly. You can configure a few parameters to get different results for your prompts.

當使用提示時，您將通過API或直接與LLM進行互動。您可以配置一些引數以獲得不同的提示結果。

**Temperature** - In short, the lower the temperature the more deterministic the results in the sense that the highest probable next token is always picked. Increasing the temperature could lead to more randomness encouraging more diverse or creative outputs. We are essentially increasing the weights of the other possible tokens. In terms of application, we might want to use a lower temperature for something like fact-based QA to encourage more factual and concise responses. For poem generation or other creative tasks, it might be beneficial to increase the temperature.

**溫度** - 簡而言之，溫度越低，結果就越具有決定性，因為最有可能的下一個標記總是被選擇。增加溫度可能會導致更多的隨機性，鼓勵更多多樣化或創意性的輸出。我們本質上是增加了其他可能標記的權重。在應用方面，對於像基於事實的問答（QA）這樣的事情，我們可能希望使用較低的溫度，以鼓勵更多事實和簡潔的回答。對於詩歌產生或其他創意任務，增加溫度可能是有益的。

**Top_p** - Similarly, with top_p, a sampling technique with temperature called nucleus sampling, you can control how deterministic the model is at generating a response. If you are looking for exact and factual answers keep this low. If you are looking for more diverse responses, increase to a higher value.

**Top_p** - 同樣地，使用 top_p，一種帶有溫度的取樣技術，稱為 nucleus 取樣，您可以控制模型在產生響應時的確定性。如果您正在尋找確切和事實性的答案，請將其保持較低。如果您正在尋找更多樣化的響應，請增加到更高的值。

The general recommendation is to alter one, not both.

一般建議只更改其中一個，而不是兩個都更改。

Before starting with some basic examples, keep in mind that your results may vary depending on the version of LLM you are using.

在開始一些基本範例之前，請記住您使用的 LLM 版本可能會影響您的結果。

---

## Standard Prompts

## 標準提示

We have tried a very simple prompt above. A standard prompt has the following format:

我們剛才使用了一個非常簡單的提示。標準提示的格式如下：

```
<Question>?
```

This can be formatted into a QA format, which is standard in a lot of QA dataset, as follows:

這可以按照問答格式進行格式化，這在許多問答資料集中是標準的，如下所示：

```
Q: <Question>?
A:
```

Given the standard format above, one popular and effective technique for prompting is referred to as few-shot prompting where we provide exemplars. Few-shot prompts can be formatted as follows:

鑒於上述的標準格式，一種流行且有效的提示技術被稱為少樣本提示，其中我們提供範例。少樣本提示可以按以下格式進行格式化：

```
<Question>?
<Answer>

<Question>?
<Answer>

<Question>?
<Answer>

<Question>?

```

And you can already guess that its QA format version would look like this:

而且你已經可以猜到它的 QA 格式版本會長這樣：

```
Q: <Question>?
A: <Answer>

Q: <Question>?
A: <Answer>

Q: <Question>?
A: <Answer>

Q: <Question>?
A:
```

Keep in mind that it's not required to use QA format. The format depends on the task at hand. For instance, you can perform a simple classification task and give exemplars that demonstrate the task as follows:

請記住，並非必須使用QA格式。格式取決於手頭的任務。例如，您可以執行簡單的分類任務，並提供以下示範以示範該任務：

*Prompt:*

*提示:*

```
This is awesome! // Positive
This is bad! // Negative
Wow that movie was rad! // Positive
What a horrible show! //
```

*Output:*

*輸出:*

```
Negative
```

Few-shot prompts enable in-context learning which is the ability of language models to learn tasks given only a few examples. We will see more of this in action in the upcoming guides.

Few-shot提示使上下文學習成為可能，即語言模型僅透過少量示範即可學習任務的能力。我們將在即將推出的指南中看到更多此類情況。

---

## Elements of a Prompt

## 提示的元素

As we cover more and more examples and applications that are possible with prompt engineering, you will notice that there are certain elements that make up a prompt.

隨著我們涵蓋越來越多的例子和應用，您會注意到構成提示的某些元素。

A prompt can contain any of the following components:

提示符可以包含以下任何元件：

**Instruction** - a specific task or instruction you want the model to perform

**指示** - 您希望模型執行的特定任務或指令

**Context** - can involve external information or additional context that can steer the model to better responses

**背景** - 可能涉及外部資訊或附加上下文，可以引導模型做出更好的響應。

**Input Data** - is the input or question that we are interested to find a response for

**輸入資料** - 是我們有興趣找到回答的輸入或問題

**Output Indicator** - indicates the type or format of the output.

**輸出指標** - 表示輸出的型別或格式。

Not all the components are required for a prompt and the format depends on the task at hand. We will touch on more concrete examples in upcoming guides.

不是所有的元件都需要用於提示，格式取決於手頭的任務。我們將在即將推出的指南中提供更具體的例子。

---

## General Tips for Designing Prompts

## 設計提示的一般建議

Here are some tips to keep in mind while you are designing your prompts:

以下是在設計提示時需要記住的一些提示：

### Start Simple

### 開始簡單

As you get started with designing prompts, you should keep in mind that it is an iterative process that requires a lot of experimentation to get optimal results. Using a simple playground like OpenAI's or Cohere's is a good starting point.

當你開始設計提示時，應該牢記這是一個迭代的過程，需要大量的實驗才能獲得最佳結果。使用像OpenAI或Cohere這樣的簡單遊樂場是一個不錯的起點。

You can start with simple prompts and keep adding more elements and context as you aim for better results. Versioning your prompt along the way is vital for this reason. As we read the guide you will see many examples where specificity, simplicity, and conciseness will often give you better results.

你可以從簡單的提示開始，隨著你追求更好的結果，不斷新增更多的元素和上下文。因此，沿途對你的提示進行版本控制非常重要。當你閱讀本指南時，你會看到許多例子，其中特定性、簡潔性和簡明性通常會給你帶來更好的結果。

When you have a big task that involves many different subtasks, you can try to break down the task into simpler subtasks and keep building up as you get better results. This avoids adding too much complexity to the prompt design process at the beginning.

當你有一個涉及許多不同子任務的大任務時，你可以嘗試將任務分解為更簡單的子任務，並隨著獲得更好的結果而不斷建構。這可以避免在開始時將太多複雜性新增到提示設計過程中。

### The Instruction

### 指令

You can design effective prompts for various simple tasks by using commands to instruct the model what you want to achieve such as "Write", "Classify", "Summarize", "Translate", "Order", etc.

您可以使用命令來指示模型實現您想要的目標，例如“寫入”，“分類”，“總結”，“翻譯”，“排序”等，從而為各種簡單任務設計有效的提示。

Keep in mind that you also need to experiment a lot to see what works best. Try different instructions with different keywords, contexts, and data and see what works best for your particular use case and task. Usually, the more specific and relevant the context is to the task you are trying to perform, the better. We will touch on the importance of sampling and adding more context in the upcoming guides.

請記住，您還需要進行大量實驗，以檢視哪種方法最有效。嘗試使用不同的指令、關鍵詞、上下文和資料，看看哪種方法最適合您的特定使用案例和任務。通常，上下文越具體和相關，對您要執行的任務越有幫助。在即將推出的指南中，我們將談到抽樣和新增更多上下文的重要性。

Others recommend that instructions are placed at the beginning of the prompt. It's also recommended that some clear separator like "###" is used to separate the instruction and context.

其他人建議將指示放在提示的開頭。同時建議使用一些清晰的分隔符，例如“###”，來區分指示和內容。

For instance:

例如：

*Prompt:*

*提示:*

```
### Instruction ###
Translate the text below to Spanish:

Text: "hello!"
```

*Output:*

*輸出:*

```
¡Hola!
```

### Specificity

### 特異性

Be very specific about the instruction and task you want the model to perform. The more descriptive and detailed the prompt is, the better the results. This is particularly important when you have a desired outcome or style of generation you are seeking. There aren't specific tokens or keywords that lead to better results. It's more important to have a good format and descriptive prompt. Providing examples in the prompt is very effective to get desired output in specific formats.

非常具體地說明您希望模型執行的指令和任務。提示越詳細和詳盡，結果就越好。當您有所期望的產生結果或風格時，這一點尤其重要。沒有特定的令牌或關鍵字可以帶來更好的結果。擁有良好的格式和詳細的提示更為重要。在提示中提供示範非常有效，可以獲得特定格式的期望輸出。

When designing prompts you should also keep in mind the length of the prompt as there are limitations regarding how long this can be. Thinking about how specific and detailed you should be is something to consider. Too many unnecessary details are not necessarily a good approach. The details should be relevant and contribute to the task at hand. This is something you will need to experiment with a lot. We encourage a lot of experimentation and iteration to optimize prompts for your applications.

在設計提示時，您還應考慮提示的長度，因為提示的長度有限制。考慮提示的具體和詳細程度是需要考慮的事情。過多的不必要細節並不一定是一個好的方法。細節應該是相關的，並有助於完成任務。這是您需要大量嘗試的事情。我們鼓勵大量的實驗和迭代，以優化您的應用程序的提示。

As an example, let's try a simple prompt to extract specific information from a piece of text.

作為一個例子，讓我們嘗試一個簡單的提示，從一段文字中提取特定的資訊。

*Prompt:*

*提示:*

```
Extract the name of places in the following text.

Desired format:
Place: <comma_separated_list_of_company_names>

Input: "Although these developments are encouraging to researchers, much is still a mystery. “We often have a black box between the brain and the effect we see in the periphery,” says Henrique Veiga-Fernandes, a neuroimmunologist at the Champalimaud Centre for the Unknown in Lisbon. “If we want to use it in the therapeutic context, we need to understand the mechanism."
```

*Output:*

*輸出:*

```
Place: Champalimaud Centre for the Unknown, Lisbon
```

The input text is obtained from [this Nature article](https://www.nature.com/articles/d41586-023-00509-z).

輸入文字來自於[這篇自然期刊文章](https://www.nature.com/articles/d41586-023-00509-z)。

### Avoid Impreciseness

### 避免不精確

Given the tips above about being detailed and improving format, it's easy to fall into the trap of wanting to be too clever about prompts and potentially creating imprecise descriptions. It's often better to be specific and direct. The analogy here is very similar to effective communication -- the more direct, the more effective the message gets across.

鑒於上述有關詳細和改進格式的提示，很容易陷入想要在提示方面過於聰明並可能建立不精確描述的陷阱。更具體和直接往往更好。這裡的類比非常類似於有效的溝通 - 越直接，資訊傳達就越有效。

For example, you might be interested in learning the concept of prompt engineering. You might try something like:

例如，您可能有興趣學習提示工程的概念。您可以嘗試類似以下的內容：

```
Explain the concept of prompt engineering. Keep the explanation short, only a few sentences, and don't be too descriptive.
```

It's not clear from the prompt above how many sentences to use and what style. You might still somewhat get good responses with the above prompts but the better prompt would be one that is very specific, concise, and to the point. Something like:

上面的提示不清楚要使用多少句子和什麼風格。您可能仍然可以使用上面的提示獲得一些好的回應，但更好的提示應該是非常具體、簡潔和直接的。例如：

```
Use 2-3 sentences to explain the concept of prompt engineering to a high school student.
```

### To do or not to do?

### 做還是不做？

Another common tip when designing prompts is to avoid saying what not to do but say what to do instead. This encourages more specificity and focuses on the details that lead to good responses from the model.

另一個常見的提示是在設計提示時避免說什麼不要做，而是說應該做什麼。這鼓勵更具體化，並關注導致模型良好響應的細節。

Here is an example of a movie recommendation chatbot failing at exactly what I don't want it to do because of how I wrote the instruction -- focusing on what not to do.

這裡是一個電影推薦聊天機器人的例子，它因為我寫的指示太過著重於不要做什麼而失敗了。

*Prompt:*

*提示:*

```
The following is an agent that recommends movies to a customer. DO NOT ASK FOR INTERESTS. DO NOT ASK FOR PERSONAL INFORMATION.

Customer: Please recommend a movie based on my interests.
Agent:
```

*Output:*

*輸出:*

```
Sure, I can recommend a movie based on your interests. What kind of movie would you like to watch? Do you prefer action, comedy, romance, or something else?
```

Here is a better prompt:

這裡有一個更好的提示：

*Prompt:*

*提示:*

```
The following is an agent that recommends movies to a customer. The agent is responsible to recommend a movie from the top global trending movies. It should refrain from asking users for their preferences and avoid asking for personal information. If the agent doesn't have a movie to recommend, it should respond "Sorry, couldn't find a movie to recommend today.".

Customer: Please recommend a movie based on my interests.
Agent:
```

*Output:*

*輸出:*

```
Sorry, I don't have any information about your interests. However, here's a list of the top global trending movies right now: [list of movies]. I hope you find something you like!
```

Some of the examples above were adopted from the ["Best practices for prompt engineering with OpenAI API" article.](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-openai-api)

一些以上的例子是從“[使用OpenAI API進行提示工程的最佳實踐](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-openai-api)”文章中採用的。

---

[Next Section (Basic Prompting)](./prompts-basic-usage.md)

[下一節（基本提示）](./prompts-basic-usage.md)

