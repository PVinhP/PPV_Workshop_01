---
title : "Walkthrough Amazon Bedrock Console"
date : 2025-06-17
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

Use Amazon Bedrock to build and scale generative AI applications with FMs. Amazon Bedrock is a fully managed service that makes FMs, from leading AI startups and Amazon, available through an API. You can choose from a wide range of FMs to find the model that is best suited for your use case. With the Amazon Bedrock serverless experience, you can get started quickly, privately customize FMs with your own data, and quickly integrate and deploy them into your applications by using AWS tools without having to manage any infrastructure.
1. Open [Amazon Bedrock console](https://console.aws.amazon.com/bedrock/home)
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh19.png?raw=true)
#### Activate model access

To complete this workshop, you need to enable access to Claude3, Mistral, and Llama models. Additionally, verify that the **Titan Text Embeddings V2** model is enabled; this model is typically available by default, but confirm its status and enable it if necessary. Please ensure you have access to all these required models before proceeding with the workshop.

1. At the upper-left of the Amazon Bedrock console, choose the menu (hamburger) icon. In the navigation pane, choose **Model access**, and then select **Enable specific models**.
2. Select the following LLMs:
   - Titan Text Embeddings V2
   - Claude 3.5 Sonnet
   - Claude 3 Haiku
   - Claude 3 Opus
   - Llama 3.1 8B Instruct 8B
   - Mistral 7B Instruct
3. Choose `Next`
4. Choose `Submit`

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh20.png?raw=true)


{{% notice note %}}

In the current version of this workshop environment, the use of the Claude 3 Opus model is not supported. If you encounter any errors related to Opus, you can safely ignore them and proceed without testing the Opus model. However, if you choose to deploy this application in your own AWS account, you will be able to enable the Opus model without any issues.

{{% /notice %}}

### Ask a question

1. At the upper-left of the Amazon Bedrock console, choose the menu (hamburger) icon.  
   Under **Test**, choose **Chat / Text playground**.

   You can also take this time, on the left menu, to navigate around the different options.

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/4.Activatemodelaccess/011.png?raw=true)

2. On the **Chat / Text playground**, choose **`Select model`**.
3. In the *Select model* popup, choose **Anthropic** and select **Claude 3 Haiku**.



![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/4.Activatemodelaccess/002.png?raw=true)

4. Choose **`Apply`**.

5. In the prompt text box, type question, and then choose **`Run`**.

   Sample question – `List top 10 cities of VietNam by population?`

6. The model returns a response similar to this:

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/4.Activatemodelaccess/402.png?raw=true)

**In the Configurations**, 
You can try randomness by changing parameter like temperature, Top P, Top K, and token length. You can also try with a different questions and experience with different response behavior.
- Temperature is a parameter that controls the level of randomness in the output; the higher the value, the more creative the model becomes. 
- Top P is the cumulative probability threshold for selecting candidate tokens, helping balance between logic and diversity. 
- Top K is the number of highest-probability tokens considered for the next word selection. 
- Max token length is the maximum number of tokens the model will generate in a response to control the output length and cost.

---
## 🎉 Congratulations

You can now proceed to the next task.
