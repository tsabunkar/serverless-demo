# Serverless Framework

- Build applications comprised of microservices that run in response :
  - events
- Advantages of this framework compared to traditional server based framework:
  - It can Auto-scale for you and only charge you when they run
  - This lowers the total cost of maintaining your apps
  - enabling you to build more logic, faster.
- The Framework uses new event-driven compute services
- Different Infrastructure cloud providers/vendors
  - AWS Lambda
  - Google Cloud
  - Microsoft Azure
- Out-of-box from this framework:
  - command-line tool
  - workflow automation
  - best practices for developing
  - deploying your serverless architecture

### Play with First-Serverless

- `\$ npm install -g serverless`
- Goto aws console
- IAM
- Users > Add User
  - User-name: `serverless-admin`
  - Access type: Programmatic access
  - Set permissions: Attach existing policies directly > Search & Select : AdministratorAccess
  - Create user
  - Access Key ID: `AKIAXGB___63`
  - Secret access Key : `pez0oCcd8LIT8/5___Xx6ZcKDwTNpAibGu`
- Since you have already installed serverless cli in your machine Run:
  - `$ serverless config credentials --provider aws --key <Access_Key_ID> --secret <Secret_access_Key>`
  - `$ serverless config credentials --provider aws --key AKIAIOSFODNN7EXAMPLE --secret wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`
- sudo nano ~/.aws/credentials (you should have aws cli installed locally)
  - Add your new multiple profile
    ```
    [<profile_name>]
    aws_access_key_id = AKIAXGB___63
    aws_secret_access_key = pez0oCcd8LIT8/5___Xx6ZcKDwTNpAibGu
    ```
- Create first serverless.yml file:
  - cd serverless/first-login/
  - touch serverless.yml
    ```yml
    service: first-service
    provider:
    name: aws
    runtime: nodejs12.x
    stage: dev
    profile: <profile_name>
    ```
- Deploy by specifc profile :
  - \$ cd serverless/first-login
  - `\$ serverless deploy --aws-profile dev-sls-profile`
- IN AWS, Go to s3 bucket
  - new proj added in bucket : `first-service-dev-serverlessdeploymentbucket-66s61k4akw3m`
- To Login to Dashboard for this: `\$ serverless login`
- NOTE : For above command you can use -> `serverless` or `sls`
  - ex: `\$ sls deploy --aws-profile dev-sls-profile`
- Ref:
  - https://www.npmjs.com/package/serverless
  - https://github.com/serverless/serverless/blob/HEAD/docs/providers/aws/guide/credentials.md

### Lambda function in js (Hello-world) - [Manual Creation of s3 bucket to store the .js file which holds lambda function]

- Create a Serverless file - \$ cd serverless/first-lambda
- \$ npm init --y
- Install SLS plugin for - offline mode : `$ npm i -g serverless-offline` or `npm i serverless-offline --save-dev` (Very useful to test locally before deploying to aws)
- Add a script in package.json : `"start" : "sls offline start"`
- \$ npm start --> (http://localhost:3000/)
- Now visit : GET | http://localhost:3000/dev/hello
- Which will invoke the lambda function written in ./handler.js (whenever there is event - GET request on /dev/hello)
- manually create a bucket - `sls-bucket-codebase` (Refer this bucket name in your serverless.yml) <-- **NOTE**: No need to specifiy s3 bucket name, Check ex- lambda-cli/mySecondSls serverless.yml file
- After testing locally, to deploy in aws : `sls deploy --aws-profile dev-sls-profile`
- To test this lambda function in aws(see the console.logs)
  - Lambda > Functions > first-sls-lambda-dev-foo
  - code > Test (button)
  - Configure test event > test event name: test > Save
  - Test (tab)
  - Invoke (with any input)
- To check the log of lambda function:

  - Lambda > Functions > first-sls-lambda-dev-foo
  - Monitoring tab
  - Logs > View logs in CloudWatch
  - In Log streams you can see the log file - `2021/03/07/[$LATEST]8a___`
  - Here you can see the log -->
    ```
    2021-03-07T16:46:14.114Z	6257044d-53b1-40ae-8274-740267ca187f	INFO	--Hello-world from SLS Lambda--
    ```

- Ref: https://www.serverless.com/framework/docs/providers/aws/guide/functions/

### NodeJS Serverless project using SLS CLI

- $ cd serverless/lambda-cli
- Boilerplate code using SLS : `$ sls create --template aws-nodejs --path mySecondSls`
- $ cd mySecondSls/
- $ npm init --y
- $ npm i serverless-offline --save-dev
- Add a script in package.json : `"start" : "sls offline start"`
- To test locally: $ npm start
- Visit: http://localhost:3000/dev/users/create
- To deploy : ` $ sls deploy --aws-profile dev-sls-profile`
- **NOTE**: Here we are not creating s3 bucket to upload this lambda code,but done defautly
- Theory:
  - handler.js : This file is a function that will be uploaded as a Lambda function to your AWS account
  - serverless.yml :
    - consist of all the configuration for our deployment
    - It tells Serverless what runtime to use, which account to deploy to, and what to deploy
    - In the provider object we need to add a new line of `profile: <serverlessUser>` --> tells Serverless to use the AWS credentials
- Ref: https://www.freecodecamp.org/news/complete-back-end-system-with-serverless/

### How to configure the env variable as per deployed env

- create `.env.<STAGE>` files ex-
  - For local env config: `.env.local`
  - For dev env config: `.env.dev`
  - For val env config: `.env.val`
  - For prod env config: `.env.prod`
- in each of your config files add first line as : `STAGE=dev`
- In `App.module.ts`:
  ```ts
  @Module({
    imports: [
      ConfigModule.forRoot({
        isGlobal: true,
        envFilePath: [`.env.${process.env.STAGE}`], // here defines the stage to pick
      }),
    ],
  })
  export class ApplicationModule {
    private readonly logger = new Logger(ApplicationModule.name);
    constructor() {
      this.logger.log('_________________________-');
      this.logger.log(`Configured for STAGE=${process.env.STAGE}`);
      this.logger.log('_________________________-');
    }
  }
  ```
- In Package.json file update relevant scripts like-
  ```json
    "start": "env STAGE=local nest start",
    "start:dev": "env STAGE=local nest start --watch",
  ```
- Also you can update SLS scripts in package.json:
  ```json
    "deploy:dev": "sls deploy --stage=dev",
    "deploy:val": "sls deploy --stage=val",
    "deploy:prod": "sls deploy --stage=prod",
    "package:dev": "sls package --stage=dev",
    "package:val": "sls package --stage=val",
    "package:prod": "sls package --stage=prod"
  ```
- Use the variables in serverless.yml file for ex:
  ```yml
  functions:
  main:
    handler: dist/serverless.handler
    environment:
      PGHOST: ${env:PGHOST}
      PGPORT: ${env:PGPORT}
      PGDATABASE: ${env:PGDATABASE}
      PGSCHEMA: ${env:PGSCHEMA}
      PGSSLMODE: ${env:PGSSLMODE}
      S3_EXPORT_BUCKET_NAME: ${cf:${self:custom.infraStack}.exportBucketName}
      SSM_DATABASE_USER_SECRET_ID: ${env:SSM_DATABASE_USER_SECRET_ID}
  ```

### Lambda function in Nestjs + SLS

- You can use this Repo for Nestjs + SLS, lambda function will be invoked when there is upload of new xml file in s3 bucket.
- locally you can just run `npm run start:dev`

---

#### Ref:

- For Nestjs + SLS Lambda function :
  - https://github.com/serverless/examples/tree/master/aws-node-typescript-nest
  - https://www.freecodecamp.org/news/complete-back-end-system-with-serverless/

---
