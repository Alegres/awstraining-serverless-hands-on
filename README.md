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


## Implement backend authentication
TODO: add diagram, explain API Gateway authorizer and how it connects with Cognito, etc.
Sample prompt: ...
Expected result: ...

## Implement logic & asynchronous flow
...

## Implement Bedrock and finalize
...
 
  
 
