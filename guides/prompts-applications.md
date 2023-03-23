# Prompt Applications

# 提示應用程式

In this guide, we will cover some advanced and interesting ways we can use prompt engineering to perform useful and more advanced tasks.

在本指南中，我們將介紹一些進階且有趣的方式，使用提示工程來執行有用且更進階的任務。

**Note that this section is under heavy development.**
Topics:

請注意，此部分正在大力開發中。
主題：

- [Generating Data](#generating-data)
- [Program-Aided Language Models](#pal-program-aided-language-models)
- [Python Notebooks](#python-notebooks)



---

- [產生資料](#產生資料)
- [程式輔助語言模型](#程式輔助語言模型)
- [Python 筆記本](#Python-筆記本)

---

## Generating Data

## 產生資料

LLMs have strong capabilities to generate text. Using effective prompt strategies can steer the model to produce better, more consistent, and more factual responses. LLMs can also be especially useful for generating data which is useful to run all sorts of experiments. For example, we can use it to generate quick samples for a sentiment classifier like so:

LLMs 具有強大的文字產生能力。使用有效的提示策略可以引導模型產生更好、更一致和更真實的回應。LLMs 還可以特別用於產生有用的資料，以運行各種實驗。例如，我們可以使用它來為情感分類器產生快速樣本，如下所示：

*Prompt:*

*提示:*

```
Produce 10 exemplars for sentiment analysis. Examples are categorized as either positive or negative. Produce 2 negative examples and 8 positive examples. Use this format for the examples:
Q: <sentence>
A: <sentiment>
```

*Output:*

*輸出:*

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
```

This is very useful. We use this example for a different test in another section of the guides.

這非常有用。我們會在指南的另一個部分中使用這個例子進行不同的測試。

---

## PAL (Program-Aided Language Models)

## PAL（程式輔助語言模型）

[Gao et al., (2022)](https://arxiv.org/abs/2211.10435) presents a method that uses LLMs to read natural language problems and generate programs as the intermediate reasoning steps. Coined, program-aided language models (PAL), differ from chain-of-thought prompting in that instead of using free-form text to obtain a solution it offloads the solution step to a programmatic runtime such as a Python interpreter.

[Gao等人，(2022)](https://arxiv.org/abs/2211.10435) 提出了一種使用LLMs來閱讀自然語言問題並產生程式作為中間推理步驟的方法。所謂的程式輔助語言模型(PAL)，與思維鏈提示不同，它不是使用自由形式的文本來獲得解決方案，而是將解決步驟解除安裝到程式設計執行時，例如Python直譯器。

![](../img/pal.png)

![](../img/pal.png)

Let's look at an example using LangChain and OpenAI GPT-3. We are interested to develop a simple application that's able to interpret the question being asked and provide an answer by leveraging the Python interpreter.

讓我們來看一個使用 LangChain 和 OpenAI GPT-3 的例子。我們有興趣開發一個簡單的應用程式，透過利用 Python 解釋器來解釋所提出的問題並提供答案。

Specifically, we are interested to create a function that allows the use of the LLM to answer questions that require date understanding. We will provide the LLM a prompt that includes a few exemplars that are adopted from [here](https://github.com/reasoning-machines/pal/blob/main/pal/prompt/date_understanding_prompt.py).

具體而言，我們有興趣建立一個函式，使得可以使用LLM來回答需要日期理解的問題。我們將向LLM提供一個提示，其中包含一些從[這裡](https://github.com/reasoning-machines/pal/blob/main/pal/prompt/date_understanding_prompt.py)採用的示範。

These are the imports we need:

這些是我們需要的匯入專案：

```python
import openai
from datetime import datetime
from dateutil.relativedelta import relativedelta
import os
from langchain.llms import OpenAI
from dotenv import load_dotenv
```

Let's first configure a few things:

讓我們先配置一些東西：

```python
load_dotenv()

# API configuration
openai.api_key = os.getenv("OPENAI_API_KEY")

# for LangChain
os.environ["OPENAI_API_KEY"] = os.getenv("OPENAI_API_KEY")
```

Setup model instance:

保設模型實例：

```python
llm = OpenAI(model_name='text-davinci-003', temperature=0)
```

Setup prompt + question:

Setup提示 + 問題:

```python
question = "Today is 27 February 2023. I was born exactly 25 years ago. What is the date I was born in MM/DD/YYYY?"

DATE_UNDERSTANDING_PROMPT = """
# Q: 2015 is coming in 36 hours. What is the date one week from today in MM/DD/YYYY?
# If 2015 is coming in 36 hours, then today is 36 hours before.
today = datetime(2015, 1, 1) - relativedelta(hours=36)
# One week from today,
one_week_from_today = today + relativedelta(weeks=1)
# The answer formatted with %m/%d/%Y is
one_week_from_today.strftime('%m/%d/%Y')
# Q: The first day of 2019 is a Tuesday, and today is the first Monday of 2019. What is the date today in MM/DD/YYYY?
# If the first day of 2019 is a Tuesday, and today is the first Monday of 2019, then today is 6 days later.
today = datetime(2019, 1, 1) + relativedelta(days=6)
# The answer formatted with %m/%d/%Y is
today.strftime('%m/%d/%Y')
# Q: The concert was scheduled to be on 06/01/1943, but was delayed by one day to today. What is the date 10 days ago in MM/DD/YYYY?
# If the concert was scheduled to be on 06/01/1943, but was delayed by one day to today, then today is one day later.
today = datetime(1943, 6, 1) + relativedelta(days=1)
# 10 days ago,
ten_days_ago = today - relativedelta(days=10)
# The answer formatted with %m/%d/%Y is
ten_days_ago.strftime('%m/%d/%Y')
# Q: It is 4/19/1969 today. What is the date 24 hours later in MM/DD/YYYY?
# It is 4/19/1969 today.
today = datetime(1969, 4, 19)
# 24 hours later,
later = today + relativedelta(hours=24)
# The answer formatted with %m/%d/%Y is
today.strftime('%m/%d/%Y')
# Q: Jane thought today is 3/11/2002, but today is in fact Mar 12, which is 1 day later. What is the date 24 hours later in MM/DD/YYYY?
# If Jane thought today is 3/11/2002, but today is in fact Mar 12, then today is 3/1/2002.
today = datetime(2002, 3, 12)
# 24 hours later,
later = today + relativedelta(hours=24)
# The answer formatted with %m/%d/%Y is
later.strftime('%m/%d/%Y')
# Q: Jane was born on the last day of Feburary in 2001. Today is her 16-year-old birthday. What is the date yesterday in MM/DD/YYYY?
# If Jane was born on the last day of Feburary in 2001 and today is her 16-year-old birthday, then today is 16 years later.
today = datetime(2001, 2, 28) + relativedelta(years=16)
# Yesterday,
yesterday = today - relativedelta(days=1)
# The answer formatted with %m/%d/%Y is
yesterday.strftime('%m/%d/%Y')
# Q: {question}
""".strip() + '\n'
```

```python
llm_out = llm(DATE_UNDERSTANDING_PROMPT.format(question=question))
print(llm_out)
```

```python
exec(llm_out)
print(born)
```

This will output the following: `02/27/1998`

This will output the following: `1998年02月27日`

---

## Python Notebooks

## Python筆記本

|Description|Notebook|
|--|--|
|Learn how to use the Python interpreter in combination with the language model to solve tasks.|[Program-Aided Language Models](../notebooks/pe-pal.ipynb)|

|描述|筆記本電腦|
|--|--|
|學習如何使用Python解譯器與語言模型結合來解決任務。|[程式輔助語言模型](../notebooks/pe-pal.ipynb)|

---

More examples coming soon!

更多的例子即將推出！

[Previous Section (Advanced Prompting)](./prompts-advanced-usage.md)

[上一節（進階提示）](./prompts-advanced-usage.md)

[Next Section (ChatGPT)](./prompts-chatgpt.md)

[下一節 (ChatGPT)](./prompts-chatgpt.md)

