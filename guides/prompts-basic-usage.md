# Basic Prompting

# 基本提示

In the previous guide, we introduced and gave a basic example of a prompt.

在之前的指南中，我們介紹並提供了一個提示的基本範例。

In this guide, we will provide more examples of how prompts are used and introduce key concepts that will be important for the more advanced guides.

在本指南中，我們將提供更多關於提示如何使用的示範，並介紹對於更進階指南非常重要的關鍵概念。

Often, the best way to learn concepts is by going through examples. Below we cover a few examples of how well-crafted prompts can be used to perform all types of interesting and different tasks.

通常，學習概念的最佳方式是透過實例。以下我們涵蓋了一些例子，展示了如何使用精心製作的提示來執行各種有趣和不同的任務。

Topics:

主題：

- [Text Summarization](#text-summarization)
- [Information Extraction](#information-extraction)
- [Question Answering](#question-answering)
- [Text Classification](#text-classification)
- [Conversation](#conversation)
- [Code Generation](#code-generation)
- [Reasoning](#reasoning)
- [Python Notebooks](#python-notebooks)


---

- [文字摘要](#text-summarization)
- [信息提取](#information-extraction)
- [問答系統](#question-answering)
- [文字分類](#text-classification)
- [對話系統](#conversation)
- [代碼生成](#code-generation)
- [推理](#reasoning)
- [Python筆記本](#python-notebooks)

---

## Text Summarization

## 文字摘要

One of the standard tasks in natural language generation is text summarization. Text summarization can include many different flavors and domains. One of the most promising applications of language models is the ability to summarize articles and concepts into quick and easy-to-read summaries. Let's try a basic summarization task using prompts.

自然語言產生中的標準任務之一是文字摘要。文字摘要可以包括許多不同的風格和領域。語言模型最有前途的應用之一是將文章和概念縮短成快速且易於閱讀的摘要。讓我們使用提示進行基本的摘要任務。

Let's say I am interested to learn about antibiotics, I could try a prompt like this:

讓我們假設我有興趣學習抗生素，我可以嘗試使用這樣的提示：

*Prompt:*

*提示:*

```
Explain antibiotics

A:
```

*Output:*

*輸出:*

```
Antibiotics are a type of medication used to treat bacterial infections. They work by either killing the bacteria or preventing them from reproducing, allowing the body’s immune system to fight off the infection. Antibiotics are usually taken orally in the form of pills, capsules, or liquid solutions, or sometimes administered intravenously. They are not effective against viral infections, and using them inappropriately can lead to antibiotic resistance.
```

The "A:" is an explicit prompt format that's used in question answering. I used it here to tell the model that there is an expected further. In this example, it's not clear how this is useful vs not using it but we will leave it that for later examples. Let's just assume that this is too much information and want to summarize it further. We can instruct the model to summarize into one sentence like so:

"A:" 是一種明確的提示格式，用於問答。我在這裡使用它來告訴模型有一個預期的進一步。在這個例子中，不清楚使用它與不使用它有什麼用處，但我們將在後面的例子中留下這個問題。讓我們假設這是太多資訊，想要進一步總結。我們可以指示模型將其總結為一句話，如下所示：

*Prompt:*

*提示:*

```
Antibiotics are a type of medication used to treat bacterial infections. They work by either killing the bacteria or preventing them from reproducing, allowing the body’s immune system to fight off the infection. Antibiotics are usually taken orally in the form of pills, capsules, or liquid solutions, or sometimes administered intravenously. They are not effective against viral infections, and using them inappropriately can lead to antibiotic resistance.

Explain the above in one sentence:
```

*Output:*

*輸出:*

```
Antibiotics are medications used to treat bacterial infections by either killing the bacteria or stopping them from reproducing, but they are not effective against viruses and overuse can lead to antibiotic resistance.
```

Without paying too much attention to the accuracy of the output above, which is something we will touch on in a later guide, the model tried to summarize the paragraph in one sentence. You can get clever with the instructions but we will leave that for a later chapter. Feel free to pause here and experiment to see if you get better results.

不要太關注上面輸出的準確性，這是我們將在後面的指南中討論的內容，該模型試圖用一句話概括段落。您可以在指令上做得更聰明，但我們將在後面的章節中討論。請隨意在此處暫停並進行實驗，以檢視是否獲得更好的結果。

---

## Information Extraction

## 資訊提取

While language models are trained to perform natural language generation and related tasks, it's also very capable of performing classification and a range of other natural language processing (NLP) tasks.

雖然語言模型是訓練用於執行自然語言產生和相關任務，但它也非常適合執行分類和一系列其他自然語言處理（NLP）任務。

Here is an example of a prompt that extracts information from a given paragraph.

這裡是一個從給定段落中提取資訊的提示示範。

*Prompt:*

*提示:*

```
Author-contribution statements and acknowledgements in research papers should state clearly and specifically whether, and to what extent, the authors used AI technologies such as ChatGPT in the preparation of their manuscript and analysis. They should also indicate which LLMs were used. This will alert editors and reviewers to scrutinize manuscripts more carefully for potential biases, inaccuracies and improper source crediting. Likewise, scientific journals should be transparent about their use of LLMs, for example when selecting submitted manuscripts.

Mention the large language model based product mentioned in the paragraph above:
```

*Output:*

*輸出:*

```
The large language model based product mentioned in the paragraph above is ChatGPT.
```

There are many ways we can improve the results above, but this is already very useful.

有很多方法可以改善上述的結果，但這已經非常有用了。

By now it should be obvious that you can ask the model to perform different tasks by simply instructing it what to do. That's a powerful capability that AI product builders are already using to build powerful products and experiences.

到目前為止，您應該已經明白，您可以透過簡單地指示模型要做什麼來要求模型執行不同的任務。這是一種強大的能力，AI 產品建造者已經在使用它來構建強大的產品和體驗。

Paragraph source: [ChatGPT: five priorities for research](https://www.nature.com/articles/d41586-023-00288-7)

[ChatGPT: 研究的五個優先事項](https://www.nature.com/articles/d41586-023-00288-7)

---

## Question Answering

## 問答

One of the best ways to get the model to respond to specific answers is to improve the format of the prompt. As covered before, a prompt could combine instructions, context, input, and output indicators to get improved results. While these components are not required, it becomes a good practice as the more specific you are with instruction, the better results you will get. Below is an example of how this would look following a more structured prompt.

其中一個讓模型回應特定答案的最佳方法是改善提示的格式。如之前所述，提示可以結合指示、內容、輸入和輸出指標以獲得更好的結果。雖然這些元件不是必需的，但隨著指示越具體，結果就會越好，這成為了一種良好的實踐。以下是一個更結構化提示的範例。

*Prompt:*

*提示:*

```
Answer the question based on the context below. Keep the answer short. Respond "Unsure about answer" if not sure about the answer.

Context: Teplizumab traces its roots to a New Jersey drug company called Ortho Pharmaceutical. There, scientists generated an early version of the antibody, dubbed OKT3. Originally sourced from mice, the molecule was able to bind to the surface of T cells and limit their cell-killing potential. In 1986, it was approved to help prevent organ rejection after kidney transplants, making it the first therapeutic antibody allowed for human use.

Question: What was OKT3 originally sourced from?

Answer:
```

*Output:*

*輸出:*

```
Mice.
```

Context obtained from [Nature](https://www.nature.com/articles/d41586-023-00400-x).

從[Nature](https://www.nature.com/articles/d41586-023-00400-x)獲得的上下文。

---

## Text Classification

## 文字分類

So far, we have used simple instructions to perform a task. As a prompt engineer, you will need to get better at providing better instructions. But that's not all! You will also find that for harder use cases, just providing instructions won't be enough. This is where you need to think more about the context and the different elements you can use in a prompt. Other elements you can provide are `input data` or `examples`.

到目前為止，我們使用簡單的指令來執行任務。作為提示工程師，您需要提供更好的指令。但這還不是全部！您還會發現，對於更難的用例，僅提供指令是不夠的。這就是您需要更多地考慮上下文和提示中可以使用的不同元素的地方。您可以提供的其他元素包括“輸入資料”或“示範”。

Let's try to demonstrate this by providing an example of text classification.

讓我們試著透過提供文字分類的示範來示範這一點。

*Prompt:*

*提示:*

```
Classify the text into neutral, negative or positive.

Text: I think the food was okay.
Sentiment:
```

*Output:*

*輸出:*

```
Neutral
```

We gave the instruction to classify the text and the model responded with `'Neutral'` which is correct. Nothing is wrong with this but let's say that what we really need is for the model to give the label in the exact format we want. So instead of `Neutral` we want it to return `neutral`. How do we achieve this. There are different ways to do this. We care about specificity here, so the more information we can provide the prompt the better results. We can try providing examples to specify the correct behavior. Let's try again:

我們已經指示將文字進行分類，模型回應為“中性”，這是正確的。這沒有問題，但假設我們真正需要的是模型以我們想要的精確格式給出標籤。因此，我們希望它返回“neutral”而不是“Neutral”。我們可以用不同的方法來實現這一點。在這裡，我們關心的是具體性，因此我們提供的資訊越多，結果就越好。我們可以嘗試提供示範以指定正確的行為。讓我們再試一次：

*Prompt:*

*提示:*

```
Classify the text into neutral, negative or positive.

Text: I think the vacation is okay.
Sentiment: neutral

Text: I think the food was okay.
Sentiment:
```

*Output:*

*輸出:*

```
neutral
```

Perfect! This time the model returned `neutral` which is the specific label I was looking for. It seems that the example provided in the prompt helped the model to be specific in its output. To highlight why sometimes being specific is important, checkout this example and spot the problem:

太好了！這次模型返回了“中性”，這正是我要找的特定標籤。看來提示中提供的示範幫助模型在輸出方面更具體。為了突出有時候具體表述的重要性，請看這個例子並發現問題：

*Prompt:*

*提示:*

```
Classify the text into nutral, negative or positive.

Text: I think the vacation is okay.
Sentiment:
```

*Output:*

*輸出:*

```
Neutral
```

What is the problem here?

這裡有什麼問題？

---

## Conversation

## 對話

Perhaps one of the more interesting things you can achieve with prompt engineering is instructing the LLM system on how to behave, its intent, and its identity. This is particularly useful when you are building conversational systems like customer service chatbots.

也許，使用提示工程實現的更有趣的事情之一是指導LLM系統如何行動、其意圖和身份。當您建構像客戶服務聊天機器人這樣的對話系統時，這尤其有用。

For instance, let's create a conversational system that's able to generate more technical and scientific responses to questions. Note how we are explicitly telling it how to behave through the instruction. This is sometimes referred to as *role prompting*.

例如，讓我們建立一個對問題能夠產生更多技術和科學回答的對話系統。請注意，我們透過指令明確地告訴它如何行為。這有時被稱為*角色提示*。

*Prompt:*

*提示:*

```
The following is a conversation with an AI research assistant. The assistant tone is technical and scientific.

Human: Hello, who are you?
AI: Greeting! I am an AI research assistant. How can I help you today?
Human: Can you tell me about the creation of blackholes?
AI:
```

*Output:*

*輸出:*

```
Sure! Black holes are regions of spacetime where the gravitational force is so strong that nothing, not even light, can escape from it. They are created when a very massive star dies and its core collapses in on itself, forming a singularity of infinite density. The intense gravity of the singularity pulls in all the matter and radiation around it, creating the black hole.
```

Our AI research assistant sounds a bit too technical, right? Okay, let's change this behavior and instruct the system to give more accessible answers.

我們的AI研究助手聽起來有點太技術化了，對吧？好的，讓我們改變這種行為，指示系統提供更易於理解的答案。

*Prompt:*

*提示:*

```
The following is a conversation with an AI research assistant. The assistant answers should be easy to understand even by primary school students.

Human: Hello, who are you?
AI: Greeting! I am an AI research assistant. How can I help you today?
Human: Can you tell me about the creation of black holes?
AI:
```

*Output:*

*輸出:*

```
Sure! Black holes are created when a very massive star runs out of fuel and collapses in on itself. The gravity of the star is so strong that nothing, not even light, can escape its pull. This creates a point in space-time with an intense gravitational force called a black hole.
```

I think we made some progress. You can continue improving it. I am sure if you add more examples you might get even better results.

我認為我們取得了一些進展。你可以繼續改進它。我相信如果你新增更多的例子，你可能會得到更好的結果。

---

## Code Generation

## 程式碼產生

One application where LLMs are quite effective is code generation. Copilot is a great example of this. There are a vast number of code-generation tasks you can perform with clever prompts. Let's look at a few examples below.

LLMs 非常有效的一個應用是程式碼產生。Copilot 是一個很好的例子。你可以透過巧妙的提示執行大量的程式碼產生任務。讓我們看一下下面的幾個例子。

First, let's try a simple program that greets the user.

首先，讓我們嘗試一個簡單的程式，向使用者問候。

*Prompt:*

*提示:*

```
/*
Ask the user for their name and say "Hello"
*/
```

*Output:*

*輸出:*

```
let name = prompt("What is your name?");
console.log(`Hello, ${name}!`);
```

You can see that we didn't even need to specify the language to use.

你可以看到，我們甚至不需要指定要使用的語言。

Let's switch levels a bit. I want to show you how powerful LLMs can be with a little more effort in designing the prompts.

讓我們稍微提高一下層次。我想向您展示，在設計提示方面再多付出一點努力，LLMs可以有多強大。

*Prompt:*

*提示:*

```
"""
Table departments, columns = [DepartmentId, DepartmentName]
Table students, columns = [DepartmentId, StudentId, StudentName]
Create a MySQL query for all students in the Computer Science Department
"""
```

*Output:*

*輸出:*

```
SELECT StudentId, StudentName
FROM students
WHERE DepartmentId IN (SELECT DepartmentId FROM departments WHERE DepartmentName = 'Computer Science');
```

This is very impressive. In this case, we provided data about the database schema and asked it to generate a valid MySQL query.

這非常令人印象深刻。在這種情況下，我們提供了有關數據庫架構的資料，並要求它產生有效的MySQL查詢。

---

## Reasoning

## 推理

Perhaps one of the most difficult tasks for an LLM today requires some form of reasoning. Reasoning is one of the areas that I am most excited about due to the types of complex applications that can emerge from LLMs.

也許對於今天的法學碩士來說，最困難的任務之一就是需要某種形式的推理。由於法學碩士可以產生各種複雜的應用程序，因此我對推理這一領域最感興趣。

There have been some improvements in tasks involving mathematical capabilities. That said, it's important to note that current LLMs struggle to perform reasoning tasks so this requires even more advanced prompt engineering techniques. We will cover these advanced techniques in the next guide. For now, we will cover a few basic examples to show arithmetic capabilities.

有關數學能力的任務已經有了一些改進。但是，值得注意的是，目前的LLMs在執行推理任務方面仍然存在困難，因此需要更先進的提示工程技術。我們將在下一個指南中介紹這些進階技術。現在，我們將介紹一些基本的例子，以展示算術能力。

*Prompt:*

*提示:*

```
What is 9,000 * 9,000?
```

*Output:*

*輸出:*

```
81,000,000
```

Let's try something more difficult.

讓我們試試更困難的事情。

*Prompt:*

*提示:*

```
The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1.

A:
```

*Output*

*輸出*

```
No, the odd numbers in this group add up to an odd number: 119.
```

That's incorrect! Let's try to improve this by improving the prompt.

那是不正確的！讓我們透過改進提示來改善它。

*Prompt:*

*提示:*

```
The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1.

Solve by breaking the problem into steps. First, identify the odd numbers, add them, and indicate whether the result is odd or even.
```

*Output:*

*輸出:*

```
Odd numbers: 15, 5, 13, 7, 1
Sum: 41
41 is an odd number.
```

Much better, right? By the way, I tried this a couple of times and the system sometimes fails. If you provide better instruction combined with examples, it might help get more accurate results.

好多了，對吧？順便說一下，我試過幾次，系統有時會失敗。如果您提供更好的指示並結合示範，可能有助於獲得更準確的結果。

We will continue to include more examples of common applications in this section of the guide.

我們將繼續在本指南的此部分中包含更多常見應用的示範。

In the upcoming guides, we will cover even more advanced prompt engineering concepts for improving performance on all these and more difficult tasks.

在即將推出的指南中，我們將涵蓋更進階的提示工程概念，以改善在所有這些和更困難的任務上的表現。

---

## Python Notebooks

## Python筆記本

|Description|Notebook|
|--|--|
|Learn how to perform many different types of common tasks using the `openai` and `LangChain` library|[Getting Started with Prompt Engineering](../notebooks/pe-lecture.ipynb)|

|描述|筆記本電腦|
|--|--|
|學習如何使用 `openai` 和 `LangChain` 函式庫執行許多不同型別的常見任務|[開始使用提示工程](../notebooks/pe-lecture.ipynb)|

---

[Previous Section (Prompts Introduction)](./prompts-intro.md)

[前一節（提示介紹）](./prompts-intro.md)

[Next Section (Advanced Prompting Techniques)](./prompts-advanced-usage.md)

[下一節（進階提示技巧）](./prompts-advanced-usage.md)

