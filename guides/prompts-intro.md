# Prompting Introduction

# 提示介紹

Prompt engineering is a relatively new discipline for developing and optimizing prompts to efficiently use language models (LMs) for a wide variety of applications and research topics. Prompt engineering skills help to better understand the capabilities and limitations of large language models (LLMs). Researchers use prompt engineering to improve the capacity of LLMs on a wide range of common and complex tasks such as question answering and arithmetic reasoning. Developers use prompt engineering to design robust and effective prompting techniques that interface with LLMs and other tools.

提示工程是一門相對較新的學科，旨在開發和優化提示，以便有效地使用語言模型（LM）來進行各種應用和研究主題。提示工程技能有助於更好地理解大型語言模型（LLM）的能力和局限性。研究人員使用提示工程來改善LLM在各種常見和複雜任務（如問題回答和算術推理）上的能力。開發人員使用提示工程來設計與LLM和其他工具介面的穩健有效的提示技術。

This guide covers the basics of standard prompts to provide a rough idea on how to use prompts to interact and instruct large language models (LLMs).

本指南涵蓋了標準提示的基礎知識，提供了如何使用提示與大型語言模型（LLM）進行互動和指導的大致想法。

All examples are tested with `text-davinci-003` (using OpenAI's playground) unless otherwise specified. It uses the default configurations, e.g., `temperature=0.7` and `top-p=1`.

所有示範都是使用 `text-davinci-003` 進行測試的（使用 OpenAI 的 Playground），除非另有說明。它使用預設配置，例如 `temperature=0.7` 和 `top-p=1`。

Topic:

主題：

- [Prompting Introduction](#prompting-introduction)
- [提示介紹](#提示介紹)
  - [Basic Prompts](#basic-prompts)
  - [基本提示](#基本提示)
  - [A Word on LLM Settings](#a-word-on-llm-settings)
  - [LLM設定的說明](#llm設定的說明)
  - [Standard Prompts](#standard-prompts)
  - [標準提示](#標準提示)
  - [Elements of a Prompt](#elements-of-a-prompt)
  - [提示的元素](#提示的元素)
  - [General Tips for Designing Prompts](#general-tips-for-designing-prompts)
  - [設計提示的一般建議](#設計提示的一般建議)
    - [Start Simple](#start-simple)
    - [開始簡單](#開始簡單)
    - [The Instruction](#the-instruction)
    - [指示](#指示)
    - [Specificity](#specificity)
    - [特定性](#特定性)
    - [Avoid Impreciseness](#avoid-impreciseness)
    - [避免不精確](#避免不精確)
    - [To do or not to do?](#to-do-or-not-to-do)
    - [做還是不做？](#做還是不做)


---

- [基本提示](#basic-prompts)
- [關於LLM設定的說明](#a-word-on-llm-settings)
- [標準提示](#standard-prompts)
- [提示元素](#elements-of-a-prompt)
- [設計提示的一般提示](#general-tips-for-designing-prompts)

---

## Basic Prompts

## 基本提示

You can already achieve a lot with prompts, but the quality of results depends on how much information you provide it. A prompt can contain information like the `instruction` or `question` you are passing to the model and including other details such as `inputs` or `examples`.

你可以透過提示已經實現很多，但結果的品質取決於你提供的資訊量。提示可以包含諸如傳遞給模型的“指令”或“問題”之類的資訊，並包括其他細節，如“輸入”或“示範”。

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

As you can see, the language model outputs a continuation of strings that make sense give the context `"The sky is"`. The output might be unexpected or far from the task we want to accomplish.

正如您所看到的，語言模型輸出了一系列與上下文“天空是”相關的字串。輸出可能出乎意料或與我們想要完成的任務相去甚遠。

This basic example also highlights the necessity to provide more context or instructions on what specifically we want to achieve.

這個基本範例也凸顯了需要提供更多上下文或指示，以確定我們想要實現的具體目標。

Let's try to improve it a bit:

讓我們試著稍微改進它：

*Prompt:*

*提示：*

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

那樣好一些嗎？好吧，我們告訴模型完成句子，所以結果看起來要好得多，因為它完全按照我們告訴它要做的事情（“完成句子”）進行。這種設計最佳提示來指導模型執行任務的方法被稱為**提示工程**。

The example above is a basic illustration of what's possible with LLMs today. Today's LLMs are able to perform all kinds of advanced tasks that range from text summarization to mathematical reasoning to code generation.

以上的例子是對LLM今天可行性的基本說明。現今的LLM可以執行各種高階任務，從文字摘要到數學推理到代碼產生。

---

## A Word on LLM Settings

## LLM設定的說明

When working with prompts, you will be interacting with the LLM via an API or directly. You can configure a few parameters to get different results for your prompts.

當使用提示時，您將通過API或直接與LLM進行互動。您可以配置一些引數以獲取不同的提示結果。

**Temperature** - In short, the lower the temperature the more deterministic the results in the sense that the highest probable next token is always picked. Increasing temperature could lead to more randomness encouraging more diverse or creative outputs. We are essentially increasing the weights of the other possible tokens. In terms of application, we might want to use lower temperature for something like fact-based QA to encourage more factual and concise responses. For poem generation or other creative tasks it might be beneficial to increase temperature.

**溫度** - 簡而言之，溫度越低，結果就越具有決定性，因為總是選擇最可能的下一個標記。提高溫度可能會導致更多的隨機性，從而鼓勵更多多樣性或創造性的輸出。我們本質上增加了其他可能標記的權重。在應用方面，我們可能希望在基於事實的問答中使用較低的溫度，以鼓勵更多事實和簡潔的回答。對於詩歌產生或其他創意任務，增加溫度可能是有益的。

**Top_p** - Similarly, with top_p, a sampling technique with temperature called nucleus sampling, you can control how deterministic the model is at generating a response. If you are looking for exact and factual answers keep this low. If you are looking for more diverse responses, increase to a higher value.

**Top_p** - 同樣地，使用 top_p，一種帶有溫度的取樣技術，稱為核心取樣，您可以控制模型在產生響應時的決定性。如果您正在尋找確切和事實性的答案，請保持低值。如果您正在尋找更多樣化的回答，請增加到較高的值。

The general recommendation is to alter one not both.

一般建議只更改一個而不是兩個。

Before starting with some basic examples, keep in mind that your results may vary depending on the version of LLM you are using.

在開始一些基本範例之前，請記住您使用的LLM版本可能會影響您的結果。

---

## Standard Prompts

## 標準提示

We have tried a very simple prompt above. A standard prompt has the following format:

我們剛才提供了一個非常簡單的提示。標準提示的格式如下：

```
<Question>?
```

This can be formatted into a QA format, which is standard in a lot of QA dataset, as follows:

這可以按照問答格式進行格式化，這在許多問答資料集中是標準的，如下所示：

```
Q: <Question>?
A:
```

Given the standard format above, one popular and effective technique to prompting is referred to as few-shot prompting where we provide exemplars. Few-shot prompts can be formatted as follows:

鑒於上述的標準格式，一種流行而有效的提示技術被稱為 few-shot prompting，其中我們提供範例。Few-shot prompts 可以按照以下格式進行格式化：

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

而且你可以猜到它的QA格式版本會長成這樣：

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

請記住，不需要使用 QA 格式，格式取決於手頭的任務。例如，您可以執行簡單的分類任務，並給出示範該任務的範例，如下所示：

*Prompt:*

*提示：*

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

Few-shot提示使上下文學習成為可能，即語言模型僅透過少量示範即可學習任務的能力。我們將在即將推出的指南中看到更多這方面的內容。

---

## Elements of a Prompt

## 提示的元素

As we cover more and more examples and applications that are possible with prompt engineering, you will notice that there are certain elements that make up a prompt.

隨著我們越來越多地涵蓋使用提示工程可能的例子和應用，您會注意到組成提示的某些元素。

A prompt can contain any of the following components:

提示可以包含以下任何元件：

**Instruction** - a specific task or instruction you want the model to perform

**指令** - 您想要模型執行的特定任務或指令

**Context** - can involve external information or additional context that can steer the model to better responses

**文脈** - 可能涉及外部資訊或其他上下文，可以引導模型做出更好的回應。

**Input Data** - is the input or question that we are interested to find a response for

**輸入資料** - 是我們有興趣找到答案的輸入或問題

**Output Indicator** - indicates the type or format of output.

**輸出指標** - 表示輸出的型別或格式。

Not all the components are required for a prompt and the format depends on the task at hand. We will touch on more concrete examples in upcoming guides.

不是所有元件都需要用於提示，格式取決於手頭的任務。我們將在即將推出的指南中涉及更多具體示範。

---

## General Tips for Designing Prompts

## 設計提示的一般建議

Here are some tips to keep in mind while you are designing your prompts:

以下是設計提示時應注意的一些建議：

### Start Simple

### 開始簡單

As you get started with designing prompts, you should keep in mind that it is really an iterative process that requires lot of experimentation to get optimal results. Using a simple playground like OpenAI's or Cohere's is a good starting point.

當您開始設計提示時，應該記住這是一個迭代的過程，需要大量實驗才能獲得最佳結果。使用像OpenAI或Cohere這樣的簡單遊樂場是一個很好的起點。

You can start with simple prompts and keep adding more elements and context as you aim for better results. Versioning your prompt along the way is vital for this reason. As we read the guide you will see many examples where specificity, simplicity, and conciseness will often give you better results.

您可以從簡單的提示開始，隨著您追求更好的結果，不斷新增更多的元素和上下文。沿途版本化您的提示是至關重要的原因。當我們閱讀指南時，您將看到許多示範，其中特定性、簡潔性和簡潔性通常會為您帶來更好的結果。

When you have big task that involves many different subtasks, you can try to break down the task into simpler subtasks and keep building up as you get better results. This avoids adding too much complexity to the prompt design process at the beginning.

當您有一個涉及許多不同子任務的大任務時，您可以嘗試將該任務分解為更簡單的子任務，並在獲得更好的結果時不斷建構。這樣可以避免在開始時將過多的複雜性新增到提示設計過程中。

### The Instruction

### 指示

You can design effective prompts for various simple tasks by using commands to instruct the model what you want to achieve such as "Write", "Classify", "Summarize", "Translate", "Order", etc.

您可以使用命令來指示模型完成各種簡單任務，例如“寫作”，“分類”，“摘要”，“翻譯”，“排序”等，從而設計出有效的提示。

Keep in mind that you also need to experiment a lot so see what works best. Trying different instructions with different keywords, context, and data and see what works best for your particular use case and task. Usually, the more specific and relevant the context is to the task you are trying to perform, the better. We will touch on the importance of sampling and adding more context in the upcoming guides.

請記住，您還需要進行大量的實驗，以檢視哪種方法最有效。嘗試使用不同的指令、關鍵詞、上下文和資料，並檢視哪種方法最適合您的特定用例和任務。通常，上下文越具體和相關，對於您要執行的任務越好。我們將在即將推出的指南中談到抽樣和新增更多上下文的重要性。

Others recommend that instructions are placed at the beginning of the prompt. It's also recommended that some clear separator like "###" is used to separate the instruction and context.

其他人建議將說明放在提示的開頭。同時建議使用一些清晰的分隔符，如“###”，來區分說明和內容。

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

### 特定性

Be very specific about the instruction and task you want the model to perform. The more descriptive and detailed the prompt is, the better the results. This is particularly important when you have a desired outcome or style of generation you are seeking. There aren't specific tokens or keywords that lead to better results. It's more important to have a good format and descriptive prompt. In fact, providing examples in the prompt is very effective to get desired output in specific formats.

請非常具體地說明您希望模型執行的指示和任務。提示越具體和詳細，結果就越好。當您有所期望的產生結果或風格時，這尤其重要。沒有特定的標記或關鍵字可以帶來更好的結果。更重要的是擁有良好的格式和具體的提示。事實上，在提示中提供示範非常有效，可以獲得特定格式的期望輸出。

When designing prompts you should also keep in mind the length of the prompt as there are limitations regarding how long this can be. Thinking about how specific and detailed you should be is something to consider. Too many unnecessary details in not necessarily a good approach. The details should be relevant and contribute to the task at hand. This is something you will need to experiment with a lot. We encourage a lot of experimentation and iteration to optimize prompts for your applications.

在設計提示時，您還應考慮提示的長度，因為提示的長度是有限制的。考慮您應該有多具體和詳細是值得考慮的。不必要的細節過多並不一定是一個好的方法。細節應該是相關的，並有助於完成任務。這是您需要大量嘗試的內容。我們鼓勵大量實驗和迭代，以優化應用程序的提示。

As an example, let's try a simple prompt to extract specific information from a piece of text.

作為一個例子，讓我們試著從一段文字中提取特定的資訊。

*Prompt:*

*提示：*

```
Extract the name of places in the following text.

Desired format:
Place: <comma_separated_list_of_company_names>

Input: "Although these developments are encouraging to researchers, much is still a mystery. “We often have a black box between the brain and the effect we see in the periphery,” says Henrique Veiga-Fernandes, a neuroimmunologist at the Champalimaud Centre for the Unknown in Lisbon. “If we want to use it in the therapeutic context, we actually need to understand the mechanism.""
```

*Output:*

*輸出:*

```
Place: Champalimaud Centre for the Unknown, Lisbon
```

Input text is obtained from [this Nature article](https://www.nature.com/articles/d41586-023-00509-z).

輸入文字來自[這篇自然文章](https://www.nature.com/articles/d41586-023-00509-z)。

### Avoid Impreciseness

### 避免不精確

Given the tips above about being detailed and improving format, it's easy to fall into the trap of wanting to be too clever about prompts and potentially creating imprecise descriptions. It's often better to be specific and direct. The analogy here is very similar to effective communication -- the more direct, the more effective the message gets across.

考慮到上述關於詳細說明和改善格式的提示，很容易陷入想要在提示方面太聰明而可能建立不精確的描述的陷阱。具體和直接往往更好。這裡的類比非常類似於有效溝通——越直接，資訊傳達就越有效。

For example, you might be interested in learning the concept of prompt engineering. You might try something like:

例如，您可能有興趣學習提示工程的概念。您可以嘗試類似以下的內容：

```
Explain the concept prompt engineering. Keep the explanation short, only a few sentences, and don't be too descriptive.
```

It's not clear from the prompt above how many sentences to use and what style. You might still somewhat get good responses with the above prompts but the better prompt would be one that is very specific, concise, and to the point. Something like:

以上提示中並未明確指出要使用多少句子以及什麼風格。您可能仍然可以透過上述提示獲得一些良好的回應，但更好的提示應該是非常具體、簡潔和直接的。例如：

```
Use 2-3 sentences to explain the concept of prompt engineering to a high school student.
```

### To do or not to do?

### 做還是不做？

Another common tip when designing prompts is to avoid saying what not to do but say what to do instead. This encourages more specificity and focus on the details that lead to good responses from the model.

另一個設計提示是避免說什麼不要做，而是說應該做什麼。這樣可以鼓勵更具體的描述和關注細節，從而得到更好的模型響應。

Here is an example of movie recommendation chatbot failing at exactly what I don't want it to do because of how I wrote the instruction -- focusing on what not to do.

這裡有一個電影推薦機器人的例子，它在我不希望它做的事情上失敗了，因為我寫的指示太著重於不要做什麼。

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

*提示：*

```
The following is an agent that recommends movies to a customer. The agent is responsible to recommend a movie from the top global trending movies. It should refrain from asking users for their preferences and avoid asking for personal information. If the agent doesn't have a movie to recommend, it should respond "Sorry, couldn't find a movie to recommend today.".

Customer: Please recommend a movie based on my interests.
Agent:
```

*Output:*

*輸出：*

```
Sorry, I don't have any information about your interests. However, here's a list of the top global trending movies right now: [list of movies]. I hope you find something you like!
```

Some of the examples above were adopted from the ["Best practices for prompt engineering with OpenAI API" article.](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-openai-api)

一些以上的例子是從["使用OpenAI API進行提示工程的最佳實踐"文章](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-openai-api)中採用的。

---

[Next Section (Basic Prompting)](./prompts-basic-usage.md)

[下一節 (基本提示)](./prompts-basic-usage.md)

