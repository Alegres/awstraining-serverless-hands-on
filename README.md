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


## Implementation
### Create & configure front-end project

#### Setup Cognito User Pool
...

### Create & configure backend project
...

### Implement backend authentication
TODO: add diagram, explain API Gateway authorizer and how it connects with Cognito, etc.
Sample prompt: ...
Expected result: ...

### Implement logic & asynchronous flow
...

### Implement Bedrock and finalize
...
 
  
 
