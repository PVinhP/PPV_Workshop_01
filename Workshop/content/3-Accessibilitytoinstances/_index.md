---
title : "Deploy Amazon Bedrock Serverless Application"
date : 2025-06-17
weight : 3
chapter : false
pre : " <b> 3. </b> "
---
In this task, you will build and deploy both the backend and frontend components of the application. The backend is deployed as a serverless application using AWS SAM, which creates an Amazon API Gateway, the necessary AWS Lambda functions, a Cognito user pool, and stores login credentials in AWS Secrets Manager.

The frontend is built using Vue.js and deployed using AWS Amplify. The frontend UI will call REST-based API calls hosted using Amazon API Gateway, API Gateway invokes AWS Lambda function, function calls Amazon Bedrock APIs. This serverless architecture allows the application to scale efficiently and reduces the operational overhead of managing the underlying infrastructure. The deployment process is automated through a startup.sh script, which handles the end-to-end provisioning of the entire application stack.

### Build and deploy the chatbot application
From the VSCode editor, run the following command to set a key environment variable used throughout the workshop. Replace `<chatbot-startup-stack>` with the actual stack name from the AWS CloudFormation console.

This CFNStackName variable will be referenced later when interacting with the resources provisioned as part of app deployment. And, the environment variable S3BucketName will be used throughout the workshop to store various data sources and knowledge bases required for the application. The backend services will interact with the contents of this S3 bucket to retrieve and process the necessary information for the application's functionality. Ensuring the S3BucketName environment variable is properly set will allow the workshop tasks to seamlessly access and utilize the required data stored in this central location.


````bash
export CFNStackName="chatbot-startup-stack"
export S3BucketName=$(aws cloudformation describe-stacks --stack-name ${CFNStackName} \
  --query "Stacks[0].Outputs[?OutputKey=='S3BucketName'].OutputValue" \
  --output text)

````
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh8.png?raw=true)

To clone the source code for this workshop, run the following command. The application code is hosted in the open-source aws-samples GitHub repository, which contains a variety of sample projects provided by AWS. Cloning the repository will ensure you have the latest version of the code and can easily make any modifications as you progress through the workshop tasks.

````bash
cd ~/environment
git clone https://github.com/aws-samples/bedrock-serverless-workshop.git
````
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh9.png?raw=true)
To build and deploy backend and frontend of serverless app, run the following command. The aws-creds.py script is used to create an AWS profile, which is a required step for the subsequent frontend deployment process..

````bash
cd ~/environment/bedrock-serverless-workshop
python3 aws-creds.py
chmod +x startup.sh
./startup.sh
````
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh11.png?raw=true)
The script is doing following tasks:

1. Upgrade OS, install jq software.
2. Build backend using sam build.
3. Deploy backend using sam deploy.
4. Install Amplify and build frontend.
5. Publish the frontend application using Amplify.
The script will take anywhere from 2 to 5 minutes to finish.

{{% notice note %}}
If there is a git alert popup window at some point, choose OK.

{{% /notice %}}

{{% notice note %}}

During amplify add host, you are prompted with a selection twice, keep the default selections and hit enter. The defaults are, Hosting with Amplify Console (Managed hosting with custom domains, Continuous deployment) for the first option, and Manual deployment for the second option.

{{% /notice %}}
After completion, you will see the following image.

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh12.png?raw=true)

---
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh13.png?raw=true)

---
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh14.png?raw=true)

---
Copy and store the value for amplifyapp url, user_id, and password in a text editor. You use these credentials to sign in to the UI.

Launch the amplifyapp url from the web browser, login using above credentials. After a successful login you see the the below home screen. Note, it is not yet ready with source documents, and the chat is not yet functional. In the next task you complete indexing your source documents and test with sample questions.
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh15.png?raw=true)
Before you move to next task, run below commands, these are the environment variables required for SAM and Amplify build commands for rest of the lab.

````bash
export CFNStackName="chatbot-startup-stack"
export AWS_REGION=us-east-1

export BedrockApiUrl=$(aws cloudformation describe-stacks --stack-name ${CFNStackName} \
  --query "Stacks[0].Outputs[?OutputKey=='ApiUrl'].OutputValue" --output text)

export BedrockApiKey=$(aws cloudformation describe-stacks --stack-name ${CFNStackName} \
  --query "Stacks[0].Outputs[?OutputKey=='ApiKey'].OutputValue" --output text)

export BedrockUserPoolId=$(aws cloudformation describe-stacks --stack-name ${CFNStackName} \
  --query "Stacks[0].Outputs[?OutputKey=='UserPoolId'].OutputValue" --output text)

export UserPoolClientId=$(aws cloudformation describe-stacks --stack-name ${CFNStackName} \
  --query "Stacks[0].Outputs[?OutputKey=='UserPoolClientId'].OutputValue" --output text)

export SecretName=$(aws cloudformation describe-stacks --stack-name ${CFNStackName} \
  --query "Stacks[0].Outputs[?OutputKey=='OpenAISecretName'].OutputValue" --output text)

export PATH=~/.npm-global/bin:$PATH
````
You have completed the deployment of the backend and frontend of the chatbot using Amazon Bedrock serverless