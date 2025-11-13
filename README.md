# Hands-on

## Prerequisites
* Create non-root user:
  * https://github.com/Alegres/awstraining-basics-hands-on?tab=readme-ov-file#create-non-root-user
* Install **(preferably run all commands from Git Bash to avoid issues!):**
  * Git Bash
  * AWS CLI
  * Node Package Manager
  * Python & Pip
  * Virtualenv Manager
  * Visual Studio Code with Copilot

Make sure that your Path contains the following NPM reference:
```bash
C:\Users\[YOUR_CORP_ID]\AppData\Roaming\npm
```

If you cannot edit your Path, request for it at the IT help, or user alternative tools like Power Tools.

Then run following commands, to make sure that everything will work during the training:

Install AWS Amplify:
```bash
npm install -g @aws-amplify/cli
```

Run configure just to confirm it is working (and exit / cancel right away):
```bash
amplify configure
```

**Attention!** If there is an error:

<img width="701" height="82" alt="image" src="https://github.com/user-attachments/assets/9eba646b-70d6-41ef-ab80-d0cd21b178c6" />

then run:
```bash
npm uninstall -g @aws-amplify/cli
```

and:
```bash
npm install -g @aws-amplify/cli
```

**Attention!** If your NPM is configured to use your project's repository, then you might have troubles while downloading the required dependencies.
In such case, please set the official NPM public repository in your installation commands like this:
```bash
npm install -g @aws-amplify/cli --registry https://registry.npmjs.org/
```

Ensure pip & virtualenv are installed:

```bash
python -m ensurepip --upgrade
```

```bash
python -m pip install --upgrade pip
```

```bash
python -m pip install --upgrade virtualenv
```

**Attention!** Python must also be added to your env Path to make it work!

Install CDK:
```bash
npm install -g aws-cdk@2.1006.0
```

**Attention!** Version 2.1006.0 is very important here, as newer versions are sometimes causing troubles!


## Setup Front & Backend Projects
### Create & configure front-end project
Create a separate space for front-end (with Git Bash):
```bash
mkdir serverless-front
```
Open the folder in Visual Studio Code, and open **Git Bash terminal.** (NOT POWERSHELL!!)

Make sure you are in your serverless-front directory:
```bash
cd serverless-front
```

Initialize new React application project in this directory:
```bash
npx create-react-app .
```

Configure Amplify:
```bash
amplify configure
```

Signin to your AWS account (technical user).

**Select region eu-central-1** and follow further instruction:
* https://docs.amplify.aws/gen1/javascript/tools/cli/start/set-up-cli/#configure-the-amplify-cli

Go to IAM -> Users and create a new user. Name it amplify-dev. **Do not grant access to the AWS Console.** (it should be a simple technical user)

Attach **AdministratorAccess-Amplify** to that user. Review and confirm user creation.

Then, click your new user name and go to Security Credentials. **Create pair of access keys** for the Command Line Interface (CLI).

Save access & secret keys somewhere, so you do not loose access to them.

**Press Enter** in the bash console to continue.

Provide **accessKeyId** and **secretAccessKey**, and confirm. Provide profile name **amplify-dev.**

Amplify will automatically create a new profile in your **.aws\credentials** file.

Initialize app:
```bash
amplify init
```

**Chooose Amplify Gen 1.** Confirm with "Prefer not to answer".

Enter default name for the project.

Initialize the project with default configuration.

**As authentication method choose AWS profile** and select previously created **amplify-dev** profile.

Amplify will deploy now the Cloud Formation stack allowing future deployments & hosting.

You can agree or disagree on Amplify improvements at the end.

#### Setup Cognito User Pool (Authentication)
Expected result:
* Created centralized user pool (in Cognito), that will keep registered users and allow registration & login

Add authentication module:
```bash
amplify add auth
```

Choose **Default configuration** at first. Then choose "Username".

Confirm with **No, I am done.**

Then, you must run:
```bash
amplify update auth
```

Select **Create or update Cognito user pool groups** and select name for your new user group. Call it **AmplifyUsers**.

Answer with "N" to the question if you want to add another group. Confirm sorting with enter.

Then, you should run the following command:
```bash
amplify push
```

Confirm that you are sure that you want to continue.

In order to add authentication, Amplify will create a Cognito User Pool, that we will later link to our API Gateway (in the backend module).

Our **amplify-dev** does not have the required permissions at first, therefore, please assign **AdministratorAccess** to that user for a moment.

**Amplify will create Cognito User pool** and link it with our application. Now you can remove the **AdministratorAccess** from the Cognito user.

**Now, we need to go to AWS console, and copy this pool's ARN**. This ARN will be needed later, as we connect our backend authentication to Cognito. Make sure to save this ARN nearby!
<img width="1904" height="506" alt="image" src="https://github.com/user-attachments/assets/6a2cd4d8-0d6e-4c77-a47a-647cfd30204f" />


### Create & configure backend project
Create a directory for backend:
```bash
mkdir serverless-backend
```

Open directory in Visual Studio Code and make sure that you are in Git Bash terminal, and that you are located in your new directory:
```
cd serverless-backend
```

Initialize CDK (we will use Python):
```bash
cdk init app --language python
```

Activate project's virtual environment:
```bash
source .venv/Scripts/activate
```

Install required Python's dependencies:
```bash
python -m pip install -r requirements.txt
```

Make sure that you ```C:\Users\[CORPID]\.aws\config``` file has the correct region set (we should work in **eu-central-1**):
```
[default]
region = eu-central-1
output = json
```

You can also export **AWS_REGION:**
```bash
export AWS_REGION=eu-central-1
```

Bootstrap CDK to your account:
```bash
cdk bootstrap --profile [PROFILE]
```

**Attention!** This time it is not about your Amplify user profile (which is responsible for front deployments), but about your non-root user created at the very begining as a training prerequisite!

Run synthesis:
```bash
cdk synth
```

Deploy the infrastructure:
```
cdk deploy --profile [PROFILE]
```

You can now go to AWS Console and confirm that your stack has been created in Cloud Formation. It should be empty for now:
<img width="1882" height="421" alt="image" src="https://github.com/user-attachments/assets/31eb4c14-b71e-4f95-840b-5c474125301f" />


## Setup guardrails for agentic mode for backend
Expected result:
* Markdown file with instructions that will set the guardrails in our CDK Python backend project

We can ask Copilot to create a markdown file with instructions for us. Make sure that you are in your backend project in Visual Studio Code.

Please start the agentic mode.

Sample prompt:
```
Please create a Copilot instruction markdown file that will set guardrails for my CDK backend infrastructure project written in Python. I want to make sure that my infrastructure code is written with all best practices in terms of the clean code and modules separation. At the same time it should be kept simple. Prepare all architectural guardrails so that my instructions can be used by my Visual Studio Code agent.
```

**ATTENTION!** Make sure that your **copilot-instructions.md** file is placed under **.github/** directory, otherwise it won't work!

## Implement backend authentication
Expected result:
* Implemented API Gateway
* Implemented Cognito authorizer
* Cognito authorizer attached to the API Gateway

We will be implementing this part, so the authorizer attached to API Gateway and Cognito:
<img width="563" height="283" alt="image" src="https://github.com/user-attachments/assets/052aefc7-0f77-466f-b59a-0814d87d8adc" />

Remember, for our agentic task to work, we should come up with a solid prompt with a good context.
Sample prompt:
```
Implement simple RestApi API Gateway with standard CORS options for CRUD and allowed authorization header. Implement Cognito authorizer and attach it to this API Gateway. Do not create Cognito User pool, but retrieve the Cognito User pool from my AWS account based on ARN I provide.
```

Sample VS Code output:
<img width="1902" height="1031" alt="image" src="https://github.com/user-attachments/assets/885b1dd3-2b21-4d8f-9d38-43aa7ca5f649" />

Keep in mind, that agent should constantly run validations (tests), and be able to correct his mistakes automatically!

Now, you can also tell agent to deploy our stack to AWS using prompt.
Sample:
```
Now, please deploy my stack with AWS profile [YOUR_PROFILE] and Cognito User Pool ARN set to: [YOUR ARN].
```

In my example, agent has managed to deploy my initial API Gateway stack with authorizer, and even implemented some dummy endpoints for CRUD operations on some "items", that we can later remove:
<img width="1899" height="1026" alt="image" src="https://github.com/user-attachments/assets/3aa9672a-679d-4173-9ae2-42d3b73195ec" />

But the most important thing is that the agent has already worked in a precisely crafted boundaries.

The last thing is that you must go to the AWS Console, open your API Gateway, and deploy your API to the prod stage:
<img width="1902" height="808" alt="image" src="https://github.com/user-attachments/assets/de4a2e87-7d04-4114-a068-e933827191e4" />


Now, let's implement login & registration in Amplify.

You now **should switch your project and go to the front-end.** Open Git bash in terminal in VS Code.

Make sure, that you are in your serverless-front directory.

First, install Amplify extension to our React app:
```bash
npm install aws-amplify @aws-amplify/ui-react
```

Adjust ```src/index.js``` by adding **aws-exports:**
```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import { Amplify } from "aws-amplify"; // Import Amplify
import awsExports from "./aws-exports";

Amplify.configure(awsExports);

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```

We will now create a login & registration form and call our API Gateway.

Thaks to Amplify, we do not have to implement the form on our own. Amplify will use its own Login component and automatically connect it with our Cognito User Pool.

Go to ```src/App.js``` and implement the following code **(this is a sample front implementation that we will later extend)**:
```javascript
import { withAuthenticator, Button, TextField, Table, TableCell, TableBody, TableRow, TableHead, Alert } from '@aws-amplify/ui-react';
import '@aws-amplify/ui-react/styles.css';
import { fetchAuthSession } from 'aws-amplify/auth';
import React, { useState } from "react";

function App({ signOut, user }) {
  const TEST_URL = "https://dsm265hrp5.execute-api.eu-central-1.amazonaws.com/prod/items";


  const fetchRecommendations = async () => {
    try {
      const authToken = (await fetchAuthSession()).tokens?.idToken?.toString();

      const response = await fetch(TEST_URL, {
        method: "GET",
        headers: {
          Authorization: authToken,
        },
      });

      const data = await response.json();

      console.log("Data")
      console.log(data)
    } catch (error) {
      console.error("Failed to fetch recommendations:", error);
    }
  };

  return (
    <div className="App">
      <h1>Hello {user.username}</h1>
      <Button variation="primary" onClick={signOut}>Sign out</Button>
      <Button variation="primary" onClick={fetchRecommendations}>Load Recommendations</Button>
    </div>
  );
}

export default withAuthenticator(App);
```

Make sure to adjust the **TEST_URL** and set it to point to your sample API Gateway endpoint deployed in the **backend** module.
**Attention!** This URL can be taken from your deployed API in API Gateway:
<img width="1902" height="864" alt="image" src="https://github.com/user-attachments/assets/9ea18bd1-457c-4014-aa94-cd1fd063f1cf" />

"items" here is just an example - a sample output endpoint from my agent, a dummy endpoint that it has implemented.

Once everything is working, you can deploy your app to AWS.
```bash
amplify add hosting
```

Choose Amazon CloudFront & S3. Confirm default bucket name.

Then deploy:
```bash
amplify publish
```

Amplify will start deploying the application to AWS:
<img width="1019" height="312" alt="image" src="https://github.com/user-attachments/assets/978a1925-23f4-47ec-bfc0-df5a3c903aaa" />

Amplify will also print the output URL:
<img width="1008" height="299" alt="image" src="https://github.com/user-attachments/assets/e2efdadd-6b42-4d22-a86d-f62a4ab619f2" />

Now, let's open it and you should be able to register users & login:
<img width="1900" height="825" alt="image" src="https://github.com/user-attachments/assets/87a341fa-574a-409a-98ca-f148d24e87fe" />

**Attention!**
Take a look at the Amplify React code. The simple line:
```javascript
export default withAuthenticator(App);
```
in App.js let our application automatically introduce registration and login forms.

Everything is now connected with the Cognito Users Pool - our centralized user store :)

JWT tokens from the Cognito will be automatically fetched and attached to our API Gateway requests thanks to these lines:
```javascript
      const authToken = (await fetchAuthSession()).tokens?.idToken?.toString();

      const response = await fetch(TEST_URL, {
        method: "GET",
        headers: {
          Authorization: authToken,
        },
      });
```

After you register and login, you can click on the button and investigate the result of the API Gateway call:
<img width="1907" height="252" alt="image" src="https://github.com/user-attachments/assets/20f7bcd0-0bab-4ae1-8519-31222217ef5e" />

The error message will say, that the CORS is not enabled on the resource response. It means, that our endpoint is not returning the correct headers.
Basically, our Lambda integration should set such CORS headers as response. We will work on this in the next chapters.

## Main logic & the asynchronous flow
We now want to implement the following components & flows:
* **Recommendations table** - a DynamoDB table, that will keep recommendations for the feedback that we have received. For example: "According to the feedback that you provided, you should work on the following areas...". The entries to that table will be inserted by the **feedback consuming Lambda**, which will consume the asynchronous message from SQS, and call Amazon Bedrock to generate some meaningful recommendation.
* **Recommendations retrieving functionality** - a functionality of our system that will allow retrieving recommendations from our DynamoDB via our REST API.
  * We will implement a Lambda, that will simply return the recommendations for the user that is currently logged in from DynamoDB table.
  * The Lambda will be attached to our API gateway and it will work as the implementation for the GET operation of our REST API.
* **Recommendations deleting functionality** - a functionality that will allow deleting all recommendations that are no longer needed for the logged in user.
  * We will implement a Lambda, that will simply remove all recommendations for the given user id (user that is currently logged in).
  * The Lambda will be attached to our API gateway and it will work as the implementation for the DELETE operation of our REST API.
* **SNS topic** that will accept our asynchronous requests to trigger the recommendation process.
  * There will be a **Feedback SQS Queue** that consumes messages on that topic (is subscribed to that topic).
  * The messages arriving at the SQS queue should trigger the feedback consuming Lambda and trigger the asynchronous flow.
* **Feedback registering functionality** that will accept the text feedback from the user and generate a message to the SNS topic asking for recommendations which could be applied to the feedback that the user received.
  * We will implement a Lambda that will take feedback as an input and send message to the SNS topic.
  * The SNS topic message should contain the feedback message and the id of the user for which the recommendations should be generated.
  * The Lambda will be attached to our API Gateway as POST REST endpoint.
* **Feedback consuming functionality** that will consume asynchronous messages from SQS and call Amazon Bedrock to generate some meaningful recommendation.
  * We will implement Lambda function that will be triggered by SQS messages.
  * The function will take bedrock model id from environmental variables. We will later provide this model id to call Bedrock model.
  * The function will ask model for the recommendations and after receiving the response it will put the recommendation for the given user to the DynamoDB table.
  * Eventually, it will also delete the consumed SQS message from queue to prevent double processing.

All necessary permissions (IAM) should be granted for our Lambdas and our SQS should be able to consume from the SNS topic. CORS headers should be properly returned by our Lambdas in the response.
Cognito authorizer should be attached to our API Gateway methods. Our Lambdas should retrieve the user id from the Authorizer context. Make sure that proper logging and error handling is implemented in the Lambda functions.

### Implementation
Now, is the description of the task precise enough to take it as an input for our agent?

Your task is to take this description, refine it (if needed, if you see something that might be important), prepare a good prompt and let the agent do the work. Good luck!

For example, I have received the following output:
<img width="1894" height="1020" alt="image" src="https://github.com/user-attachments/assets/751c5839-a9cc-4ec4-81bb-5d80bac69004" />

**Remember**, that you can deploy your CDK stack again with the CDK commands.

First, activate your virtual env:
```bash
source .venv/Scripts/activate
```

and then simply run:
```bash
cdk deploy --profile [PROFILE]
```

## Configure Bedrock
In the previous chapter the agent should implement the whole backend functionality for us.
However, there is still some manual action that we have to take in AWS console. We need to **obtain the Amazon Bedrock model id.** And to do this, we first need to activate such model for us in the AWS console:

1. Go to AWS and into the Amazon Bedrock console.
2. Go to the **Model catalog** and search for the simplest, cheapest model, eg. **Nova Micro.** Make sure that the model has the **Cross-region inference**.
<img width="1872" height="764" alt="image" src="https://github.com/user-attachments/assets/772667c9-f3c9-4757-9881-90e575de4b9c" />

3. Click on that model and copy the **Model ID**. Add "eu" at the beginning (to use it in EU region more efficiently) and save it somewhere.
<img width="1902" height="750" alt="image" src="https://github.com/user-attachments/assets/7649d810-0156-4e69-9e31-860ef6043df9" />

```bash
eu.amazon.nova-micro-v1:0
```

4. Eventually, please set the model ID on the backend side and deploy your stack again.

In my case, my backend architecture is organized in such way, that all config variables are placed inside the **constants.py**. So I had to set the model id there, and also make sure that the region is set to **eu-central-1!**
<img width="1381" height="698" alt="image" src="https://github.com/user-attachments/assets/1e1ee6c4-55a9-4aff-a076-31889448e8e9" />

**ATTENTION!**
Remember to save your constants file before deployment!!

**ATTENTION 2!!**
In case CDK does not see changes in your envs, this might be due to the fact that it is treating the envs as a metadata only. In this case you can ask your agent to implement the function hashing mechanism, with a prompt like this:
```
Add MD5 hash of Lambda environment variables to the function's description field to force CloudFormation updates when only env vars change.
```

Then you can proceed with:
```bash
source .venv/Scripts/activate
```

and:
```bash
cdk deploy --profile [PROFILE]
```

CDK should show you the change:
<img width="1254" height="381" alt="image" src="https://github.com/user-attachments/assets/ec5e1d71-23b1-441d-bca7-c2491c99c313" />


## Finish front-end
The final task is to implement the functionality on the front-end side, and use the endpoints that we implemented on the backend to have a working system, where user can post the feedback as a text in a form, and then receive recommendations after, for example, clicking on the "Receive feedback" button, that will trigger the endpoint that consumes recommendations from our DynamoDB table.

Try to use the knowledge you gathered so far to implement this solution with the help of an agent!

## Known Issues
If your app does not refresh after pushing the changes, then use -c flag to invalidate CloudFront cache:

```bash
amplify publish -c
```

**ATTENTION!** Make sure that you always save your Javascript files in the VS Code!!

If you face the following error:
```bash
$ cdk synth
Traceback (most recent call last):
  File "C:\personal\awstraining-serverless\basic-backend\app\app.py", line 4, in <module>
    import aws_cdk as cdk
ModuleNotFoundError: No module named 'aws_cdk'
Subprocess exited with error 1
```

make sure that you activated your env:
```bash
source .venv/Scripts/activate
```

Also, when running cdk commands, make sure to stay at the location where your cdk.json file is present.
 
  
 
