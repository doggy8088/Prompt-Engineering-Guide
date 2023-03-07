# Miscellaneous Topics

# 雜項主題

In this section, we discuss other miscellaneous and uncategorized topics in prompt engineering. It includes relatively new ideas and approaches that will eventually be moved into the main guides as they become more widely adopted. This section of the guide is also useful to keep up with the latest research papers on prompt engineering.

在本節中，我們討論提示工程中的其他雜項和未分類主題。它包括相對較新的想法和方法，隨著它們越來越廣泛地被採用，最終將被移入主要指南中。本指南的這一部分也有助於跟上提示工程的最新研究論文。

**Note that this section is under heavy development.**

請注意，此部分仍在大力開發中。

Topic:

主題:

- [Active Prompt](#active-prompt)
- [Directional Stimulus Prompting](#directional-stimulus-prompting)
- [Program-Aided Language Models](#program-aided-language-models)
- [ReAct](#react)
- [Multimodal CoT Prompting](#multimodal-prompting)
- [GraphPrompts](#graphprompts)
- ...


---

- [主動提示](#主動提示)
- [定向刺激提示](#定向刺激提示)
- [程式輔助語言模型](#程式輔助語言模型)
- [ReAct](#ReAct)
- [多模式CoT提示](#多模式提示)
- [GraphPrompts](#GraphPrompts)
- ...

---

## Active-Prompt

## Active-Prompt

Chain-of-thought (CoT) methods rely on a fixed set of human-annotated exemplars. The problem with this is that the exemplars might not be the most effective examples for the different tasks. To address this, [Diao et al., (2023)](https://arxiv.org/pdf/2302.12246.pdf) recently proposed a new prompting approach called Active-Prompt to adapt LLMs to different task-specific example prompts (annotated with human-designed CoT reasoning).

Chain-of-thought（CoT）方法依賴於一組固定的人工標註示範。問題在於，這些示範可能不是不同任務的最有效示範。為瞭解決這個問題，[Diao et al.，(2023)](https://arxiv.org/pdf/2302.12246.pdf)最近提出了一種新的提示方法，稱為Active-Prompt，以適應LLM到不同的任務特定示範提示（使用人工設計的CoT推理進行標註）。

Below is an illustration of the approach. The first step is to query the LLM with or without a few CoT examples. *k* possible answers are generated for a set of training questions. An uncertainty metric is calculated based on the *k* answers (disagreement used). The most uncertain questions are selected for annotation by humans. The new annotated exemplars are then used to infer each question. 

下面是一種方法的說明。第一步是用或不用幾個 CoT 示範來查詢 LLM。對於一組訓練問題，產生 *k* 個可能的答案。基於 *k* 個答案計算不確定性指標（使用不一致性）。選擇最不確定的問題由人員進行註釋。然後使用新的註釋示範來推斷每個問題。

![](../img/active-prompt.png)

![](../img/active-prompt.png)

---

## Directional Stimulus Prompting

## 方向性刺激提示

[Li et al., (2023)](https://arxiv.org/abs/2302.11520) proposes a new prompting technique to better guide the LLM in generating the desired summary.

[Li et al., (2023)](https://arxiv.org/abs/2302.11520) 提出了一種新的提示技術，以更好地引導LLM產生所需的摘要。

A tuneable policy LM is trained to generate the stimulus/hint. Seeing more use of RL to optimize LLMs.

一種可調節的策略語言模型被訓練用於產生提示/線索。看到更多使用強化學習來優化低語言模型。

The figure below shows how Directional Stimulus Prompting compares with standard prompting. The policy LM can be small and optimized to generate the hints that guide a black-box frozen LLM.

下面的圖顯示了定向刺激提示與標準提示的比較。政策LM可以很小，並且可以優化以產生引導黑盒凍結LLM的提示。

![](../img/dsp.jpeg)

![](../img/dsp.jpeg)

Full example coming soon!

Full example coming soon!

---

## ReAct

## ReAct

[Yao et al., 2022](https://arxiv.org/abs/2210.03629) introduced a framework where LLMs are used to generate both reasoning traces and task-specific actions in an interleaved manner. Generating reasoning traces allow the model to induce, track, and update action plans, and even handle exceptions. The action step allows to interface with and gather information from external sources such as knowledge bases or environments.

[Yao等人，2022](https://arxiv.org/abs/2210.03629)提出了一個框架，其中LLMs被用於交替地產生推理軌跡和任務特定的動作。產生推理軌跡使模型能夠誘導、追蹤和更新行動計劃，甚至處理異常情況。動作步驟允許與外部來源（如知識庫或環境）進行介面和收集資訊。

The ReAct framework can allow LLMs to interact with external tools to retrieve additional information that leads to more reliable and factual responses.

ReAct框架可以允許LLMs與外部工具互動，以檢索額外的資訊，從而得出更可靠和事實的回答。

![](../img/react.png)

![](../img/react.png)

Full example coming soon!

Full example coming soon!

---

## Multimodal CoT Prompting

## 多模式 CoT 提示

[Zhang et al. (2023)](https://arxiv.org/abs/2302.00923) recently proposed a multimodal chain-of-thought prompting approach. Traditional CoT focuses on the language modality. In contrast, Multimodal CoT incorporates text and vision into a two-stage framework. The first step involves rationale generation based on multimodal information. This is followed by the second phase, answer inference, which leverages the informative generated rationales.

[Zhang等人（2023年）](https://arxiv.org/abs/2302.00923) 最近提出了一種多模態的思維鏈提示方法。傳統的CoT側重於語言模態。相比之下，多模態CoT將文字和視覺融入到一個兩階段的框架中。第一步涉及基於多模態資訊的理由產生。接下來是第二階段的答案推斷，利用產生的理由進行推斷。

The multimodal CoT model (1B) outperforms GPT-3.5 on the ScienceQA benchmark.

多模式CoT模型（1B）在ScienceQA基準測試中表現優於GPT-3.5。

![](../img/multimodal-cot.png)

![](../img/multimodal-cot.png)

Further reading:

Further reading: (進一步閱讀:)

- [Language Is Not All You Need: Aligning Perception with Language Models](https://arxiv.org/abs/2302.14045) (Feb 2023)


---

[Language Is Not All You Need: Aligning Perception with Language Models](https://arxiv.org/abs/2302.14045)（2023年2月）

---

## GraphPrompts

## GraphPrompts

[Liu et al., 2023](https://arxiv.org/abs/2302.08043) introduces GraphPrompt, a new prompting framework for graphs to improve performance on downstream tasks.

[Liu等人，2023年](https://arxiv.org/abs/2302.08043)介紹了GraphPrompt，一個新的圖形提示框架，以提高下游任務的效能。

More coming soon!

More coming soon!

---

[Previous Section (Reliability)](./prompts-reliability.md)

[前一節（可靠性）](./prompts-reliability.md)

