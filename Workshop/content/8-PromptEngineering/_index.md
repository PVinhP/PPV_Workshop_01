---
title : "Prompt Engineering"
date :  2025-07-17
weight : 8
chapter : false
pre : " <b> 8. </b> "
---

Prompt engineering is a crucial aspect of the Retrieval-Augmented Generation (RAG) architecture, as it plays a vital role in each stage of the process. In the Retrieval stage, prompt engineering is employed to craft effective queries that retrieve the most relevant information from data sources, such as Amazon S3 documents indexed with Amazon Kendra, accurately capturing the user's intent. During the Analysis stage, prompts guide the processing and extraction of insights from the retrieved data, specifying the type of analysis, level of detail, and desired output format. Finally, in the Generation stage, prompt engineering is essential for leveraging large language models like Claude 3 Sonnet to produce coherent, relevant, and task-specific responses, providing the necessary context, instructions, and guidelines. By mastering prompt engineering, users can optimize the performance of the RAG architecture, ensuring that the retrieved information is relevant, the analysis is insightful, and the generated responses effectively address the user's needs.

In this task, you will work on fine-tuning the prompt for the Claude 3 Sonnet large language model. You will experiment with how the response behavior changes based on the prompt template you provide. Initially, you will ask a question with a loosely structured prompt, which may cause the LLM to deviate from the context and provide undesirable output. As you refine the prompt, the response will become more closely aligned with the expected outcome and exhibit less hallucination. This section is still part of the RAG architecture and will utilize the same data sources you employed in the previous task.

1. Return to the browser and open the chatbot webpage. Click on **Prompt Engineering** link. The webpage is displayed as follows:
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/010.png?raw=true)
2. Try a below question which is not relevant to the source documents you so far indexed.
````bash
Tell me a story about a fox and tiger, the story must be for a 5 year old and under 100 words.
````
3. For the prompt template, copy the following prompt and paste it into the **Prompt Template** text box. This is a loosely structured prompt without any specific direction or guidance provided to the Claude 3.5 Sonnet model.
````bash
 {context} and {question}
````
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/011.png?raw=true)

4. You will most likely receive a response similar to the one shown below. However, this response does not exist in any of the source documents. Due to the weak prompt structure, the model has hallucinated the response, providing an output that is not grounded in the given context or information.

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/015.png?raw=true)