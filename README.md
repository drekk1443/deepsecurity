# AppSync Broadcaster

Simple app that broadcasts messages to multiple clients using Serverless GraphQL real-time subscriptions powered by AWS AppSync.

The API access is authenticated via API Key valid for 7 days. The demo takes advantage of GraphQL Subscriptions and Local Resolvers so instead of calling a remote data source, the local resolver will just forward the result to all clients subscribed to the call (PubSub). Messages are not saved or persisted anywhere.

![Screnshot](/media/broadcaster.png)

## Deploy

Click on DEPLOY from the [application page](https://serverlessrepo.aws.amazon.com/#/applications/arn:aws:serverlessrepo:us-east-1:244958302947:applications~appsync-broadcaster) to deploy the AWS AppSync backend


## React Front-End setup

1. Clone the repository https://github.com/deep-security/tdc-2021-techday-2-C1OSS 
2. In the DEPLOYMENT STATUS page, click on VIEW CLOUDFORMATION STACK or go to the CloudFormation console, click on the stack `aws-serverless-repository-appsync-broadcaster` and open the OUTPUTS tab
3. Edit the file `src/App.js` and replace the following values according to the CloudFormation Stack Outputs:

    ```javascript
    Amplify.configure({
        'aws_appsync_graphqlEndpoint': 'https://xxxxxx.appsync-api.us-east-1.amazonaws.com/graphql',
        'aws_appsync_region': 'us-east-1',
        'aws_appsync_authenticationType': 'API_KEY',
        'aws_appsync_apiKey': 'da2-xxxxxxxxxxxxxxxxxxxxxxxxxx',
    });
    ```
4. Install the required modules:

    ```bash
    npm install
    ```

5. Execute the application and access it from multiple browsers at http://localhost:3000 :

    ```bash
    npm start
    ```

6. Send a message from one client and get it broadcasted in all browsers. Since AWS AppSync automatically scales, you can have thousands of clients broadcasting messages.

## Hosting on S3 or CloudFront

1. Install the AWS Amplify CLI

   ```bash
   npm install -g @aws-amplify/cli
   ```

2. From the root of the cloned repository folder, create an Amplify project by following the prompts and select REACT as framework of choice:

    ```bash
    amplify init
    ```

3. Execute 

    ```bash
    amplify add hosting
    ``` 

    from the project's root folder and follow the prompts to create an S3 bucket (DEV) and/or a CloudFront distribution (PROD). More info on the [Amplify CLI docs](https://aws-amplify.github.io/docs/cli/hosting?sdk=js).

4. Build, deploy, upload and publish the application with a single command:

   ```bash
   amplify publish
   ```

## Hosting with the [Amplify Console](https://aws.amazon.com/amplify/console/)

1. Fork the repository https://github.com/deep-security/tdc-2021-techday-2-C1OSS to your own GitHub account
2. Commit the modified `src/App.js` file with the valid AppSync resources to the forked repository
3. Connect your repository as per the instructions on https://docs.aws.amazon.com/amplify/latest/userguide/getting-started.html
4. Deploy and access your app (xxxxxxxx.amplifyapp.com)

Access the app from multiple devices or send the S3/CloudFront/AmplifyApp.com link to friends to test Serverless GraphQL real-time subscriptions

>*For a version deployed end to end with the AWS Amplify CLI using a GraphQL Transformer that will configure a DynamoDB NoSQL table to save the broadcasted messages (this version uses direct PubSub with Local Resolvers, no messages are saved) check my other repo https://github.com/awsed/broadcaster*
