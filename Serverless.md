# Serverless

- History of Computing:
  - Physical servers
  - Virtual Machines
  - Containers
  - Serverless
- Physical servers:
  - On-premises physical / hardware server
- Virtual Machines: 
  - emulates an OS/ computer system
  - Think like a tool to be a server because it only utilizes your personal computer to run another computer like a virtual machine
  - Reliability: High, Cost: High
  - **Pros**: HyperVisor + OS makes it harder to compromise the host machine in the event the virtual machine is hijacked
  - **Cons**: 
    - Consume a  huge amount of resources from the Host machine
    - High Maintain cost,  
    - Bit trickier to deploy and orchestrate
- Containers:
  - emulate an OS, however these are run under processes directly in the host machine's OS
  - Built to be lightweight with a singular purpose
  - Reliability: High, Cost: Medium
  - Pros:
    - They can be started, stopped, frozen and overall controlled very easily with the use of orchestration, Thus allowing for more optimal usage of resources in the host machine
    - We can create a container for a specific type of operation you want and call upon it for any given task
    - Fast deployment, making it less painful to kill host machines when they aren't in use.
- Serverless
  - Serverless computing is a cloud computing execution model in which the cloud provider allocates machine resources on demand.
  - It take care of the server on behalf of  the customers/client thus, **Contrary to its name** it still involves a server
  - Reliability: High, Cost: Low-High (depends on usage)
  - Pros:
    - No need to worry about maintainability. Scalability
    - Pay by time used to execute these functions  (When an app is not in use, there are no computing resources allocated to the app. Pricing is based on the actual amount of resources consumed by an application)
    - Simplify the process of  deployment of code into production
    - It can Auto-scale for you (concurrency parameter in AWS lambda)
    - As a developer - you love to focus on logic building, solving business problems, writing re-usable services rather than scratching your head for configuring parameters related to Operations and deployment (This is exactly provided by Serverless :) 
  - Cons:
    - Poorly optimized functions or even poorly thought out requirements can quickly elevate cost

---

#### 											Serverless Runtime

- Serverless vendors offer compute runtimes, also known as function as a service (FaaS) platforms
- Famous Infrastructure cloud providers/vendors for FaaS
  - AWS -  AWS Lambda
  - GCP - Google Cloud Functions 
  - Microsoft Azure - Azure Functions
- Serverless Database
  - These systems extend the serverless execution model to the RDBMS
  - Eliminating the need to provision or scale virtualized or physical database hardware
  - Ex- AWS - Amazon Aurora, GCP - Firebase, Microsoft- Azure data lake, etc



---

##### 											Serverless Framework

- Now lets make our hand dirty on locking to our-self to specific vendor like- AWS and see how we cans use this serverless framework to take all advantages looked earlier
- https://www.serverless.com/
- This Framework - build applications comprised of microservices that run in response/triggers to **Events** (Another fancy term - **event-driven compute services**)
  - In terms of Lambda function an event can be (different ways of invoking lambda function)-
    - File uploaded, removed, etc in S3 bucket
    - Any new message received from SNS/SQS (Messaging Service)
    - API Gateway
    - Complete list is here - https://aws.amazon.com/blogs/architecture/understanding-the-different-ways-to-invoke-lambda-functions/ 
      - You can categorised into Synchronous Invoke, Asynchronous Invoke, Poll-based Invokes
- Out-of-box from this framework
  - command-line tool
  - workflow automation
  - best practices for developing
  - deploying your serverless architecture
- For Serverless Architecture - If you are looking for Boilerplate/Kick-starter/Template project  : https://github.com/serverless/examples
- Lets get our hands dirty-
  - Deploy First Lambda Function (in nodejs) using SLS <--  triggered by API Gateway
  - As a lazy developer using SLS CLI ;)
  - What is SLS Offline Plugin ?
  - Lambda function in Nestjs using SLS <==  triggered by file upload by S3 bucket
  - Lambda function in Nestjs using SLS <==  triggered by API Gateway
- (I know you had this doubt) What is difference b/w serverless lambda function & microservice ? ==> Ans: Very Small Difference !!
  - Serverless lambda function when bundled together, constitute a microservice
  - Microservices are not stateless since they run in containers, whereas- serverless functions are stateless by nature
  - Also in AWS world - Microservices can be implemented using serverless function when ==>  AWS API Gateway + Lambda Function
- At End - **Serverless is Future** :) 



---

- Reference:
  - https://en.wikipedia.org/wiki/Serverless_computing
  - https://medium.com/cafe24-ph-blog/compare-and-contrast-vm-containers-serverless-21062f3c16ed
  - https://stackoverflow.com/questions/57070054/containers-vs-serverless-vs-virtual-machines
  - https://aws.amazon.com/blogs/architecture/understanding-the-different-ways-to-invoke-lambda-functions/
  - https://www.freecodecamp.org/news/complete-back-end-system-with-serverless/
  - https://www.simform.com/serverless-architecture-guide/
  - https://stackoverflow.com/questions/45606788/relation-between-serverless-architecture-and-microservices#:~:text=Is%20the%20relation%20between%20Serverless,has%20no%20server%20scalability%20issue.

