**BUILD A SERVERLESS WEB APPLICATION**

Welcome to our serverless web app project using AWS services like Lambda, API Gateway, Amplify, DynamoDB, and Cognito. We'll build a scalable and secure solution for requesting unicorn rides. With a HTML UI, users can easily select pickup locations, interact with our RESTful backend, and register/login for a personalized experience. Let's create a modern, scalable, and secure unicorn ride service!

**Application Architecture**

The application architecture uses:

-   [AWS Lambda](https://aws.amazon.com/lambda/)
-   [Amazon API Gateway](https://aws.amazon.com/api-gateway/)
-   [Amazon DynamoDB](https://aws.amazon.com/dynamodb/)
-   [Amazon Cognito](https://aws.amazon.com/cognito/)
-   [AWS Amplify Console](https://aws.amazon.com/amplify/)

![A picture containing text, screenshot, diagram, font Description automatically generated](media/5df8da4fc30bac2dd6baf73ba7f2af72.png)

**Modules**

-   **Static Web Hosting**: AWS Amplify hosts static web resources including HTML, CSS, JavaScript, and image files which are loaded in the user's browser.
-   **User Management**: Amazon Cognito provides user management and authentication functions to secure the backend API.
-   **Serverless Backend**: Amazon DynamoDB provides a persistence layer where data can be stored by the API's Lambda function.
-   **RESTful API**: JavaScript executed in the browser sends and receives data from a public backend API built using Lambda and API Gateway.

**Module 1: Static Web Hosting**

In this module, we'll leverage AWS Amplify to easily configure continuous deployment and hosting of our web application's static resources, while also preparing for future enhancements by incorporating JavaScript to interact with remote RESTful APIs powered by AWS Lambda and Amazon API Gateway.

**Implementation**

**Select Region**: This web application can be deployed in any AWS Region that supports all the services used in this application, which include AWS Amplify, AWS CodeCommit, Amazon Cognito, AWS Lambda, Amazon API Gateway, and Amazon DynamoDB.

Select the Region from the dropdown in the upper right corner of the [AWS Management Console](https://console.aws.amazon.com/console/home).

![A screenshot of a computer Description automatically generated with medium confidence](media/e9b3a8a52193a81dc7664b360ee5441b.png)

**Create Git Repository :** We will use CodeCommit to store our application code, but you can do the same by [creating a repository](https://help.github.com/en/articles/create-a-repo) on GitHub

-   Open the AWS CodeCommit console
-   Select Create Repository
-   Set the Repository name to "wildrydes-site”.
-   Select Create

    Now that the repository is created, set up an IAM user with Git credentials in the IAM console.

    Back in the CodeCommit console, from the Clone URL dropdown, select Clone HTTPS

    ![A screenshot of a computer Description automatically generated with low confidence](media/02bd5873fdc1a3544fae4d1d62c7c284.png)

    From a terminal window run git clone and the HTTPS URL of the repository:

    git clone https://git-codecommit.eu-west2.amazonaws.com/v1/repos/wildrydes-site  
    Cloning into 'wildrydes-site'...  
    Username for 'https://git-codecommit.eu-west-2.amazonaws.com':XXXXXXXXXX  
    Password for 'USERID': XXXXXXXXXXXX  
    warning: You appear to have cloned an empty repository.

    **Populate Git Repository:** Once we've used either AWS CodeCommit or GitHub.com to create your git repository and clone it locally, we'll need to copy the website content from an existing publicly accessible S3 bucket associated with this workshop and add the content to our repository.

Change directory into our repository and copy the static files from  
S3: cd wildrydes-site  
aws s3 cp s3://wildrydes-us-east-1/WebApplication/1_StaticWebHosting/website ./ -- recursive

**Commit the files to Git service**  
git add .  
git commit -m 'new'  
git push

**Enable Web Hosting with the AWS Amplify console:** Next, we'll use the [AWS Amplify Console](https://aws.amazon.com/amplify/console/) to deploy the website you've just committed to git.

-   Launch the Amplify Console console page.
-   Click Get Started under Deploy with Amplify Console
-   Go to New App on the top right and choose Host Web App
-   Select CodeCommit under Get started with Amplify Hosting
-   Select the Repository service provider used today and select Continue.
-   If you used GitHub, you'll need to authorize AWS Amplify to your GitHub account.
-   From the dropdown, select the Repository and Branch you just created and select Next.

    ![A screenshot of a computer Description automatically generated with medium confidence](media/f43d3a333d4779ee5598e1cac892d1c4.png)

-   On the Configure build settings page, leave all the defaults, Select Allow AWS Amplify to automatically deploy all files hosted in your project root directory and select Next.
-   On the "Review" page select Save and deploy
-   The process takes a couple of minutes for Amplify Console to create the necessary resources and to deploy the code.

![A screenshot of a web page Description automatically generated with low confidence](media/955abbccd5ca2daa8322ad4e5943456d.png)

Once completed, click on the site image to launch your Wild Rydes site.

![A picture containing horse, mammal, screenshot, graphic design Description automatically generated](media/7804fc1e21d51d6aa6196f664525408e.png)

Clicking on the link for Master we'll see the build and deployment details related to our branch.

![A screenshot of a computer Description automatically generated with medium confidence](media/87595fa43eae86a54990e7edaadd35a5.png)

**Modify the website title:** To test the automatic rebuilding and redeployment process with AWS Amplify Console:

-   Open wildryde-site/index.html in a text editor on your local machine.
-   Modify the title line to \<title\>Wild Rydes - Rydes of the Future!\</title\>.
-   Save the file and commit the changes to your Git repository.
-   Amplify Console will detect the repository update and quickly begin the build process.
-   Monitor the process on the Amplify Console page to see the changes reflected in the deployed site.

![A screenshot of a computer Description automatically generated with low confidence](media/39696456d1a2d22eaf6e00bc1b0af949.png)

**Module 2: User Management**

In this module we'll create an Amazon Cognito user pool to manage users' accounts. We'll deploy pages that enable customers to register as a new user, verify their email address, and sign into the site.

**Implementation**

**Create Cognito User Pool and integrate an App to the User Pool:** Amazon Cognito offers two authentication mechanisms: Cognito User Pools, which enable you to incorporate sign-up and sign-in features into your application, and Cognito Identity Pools, which allow authentication through social identity providers or your own identity system. In this module, we will utilize a user pool as the foundation for the registration and sign-in pages provided.

-   In the AWS Console, search for and select Cognito, then choose "Create user pool."
-   Configure the user pool's sign-in options, selecting "Username" as the sign-in method.
-   Keep the default settings for other configurations such as authentication providers.
-   Proceed to configure security requirements, optionally enabling multi-factor authentication (MFA).
-   Keep the default settings for the sign-up experience configuration.
-   Configure email delivery using Amazon SES as the email provider.
-   Provide names for the user pool and initial app client, keeping other settings as default.
-   Review the details and create the user pool.
-   Take note of the Pool ID and App client ID on the Pool details page for future reference.

**Update the website config.js file:**

-   Open the wildryde-site/js/config.js file on your local machine using a text editor.
-   Update the cognito section in the file with the correct values for the user pool ID and app client ID obtained from the Amazon Cognito console.
-   Save the changes and upload the updated config.js file push it to the Git repository to have it automatically deploy to Amplify Console.

The updated config.js file

![A picture containing text, screenshot, font, line Description automatically generated](media/18218e160e2f3b3f8096b6940e7f6c89.png)

**Validate the Implementation:**

-   Access the /register.html page on your website or click on the "Giddy Up!" button on the homepage.
-   Fill out the registration form with your desired information, including a valid or fake email address, and a password that meets the specified requirements.
-   Submit the form, and you should receive an alert confirming the successful creation of your user.
-   Confirm your new user either by following the verification process in the received email (if using a controlled email address) or manually through the Cognito console.
-   In the Cognito console, select the WildRydes user pool, navigate to "Users and groups," locate the user associated with the registered email address, and confirm the user.
-   Once the user is confirmed, visit the /signin.html page and log in using the registered email address and password.
-   Upon successful login, you will be redirected to the /ride.html page.
-   We should see a notification that the API is not configured.

![A screenshot of a computer error Description automatically generated with low confidence](media/a64a34e8881979498c0273e09fccdbed.png)

**Module 3: Serverless Service Backend**

In this module, AWS Lambda and Amazon DynamoDB will be utilized to create a backend process for handling unicorn ride requests from the web application deployed in the previous module, allowing users to request and send unicorns to their desired locations.

**Implementation**

**Create an Amazon Dynamo DB table:** Create a new DynamoDB table named "Rides" in the Amazon DynamoDB console with a partition key called "RideId" of type String and take note of the generated ARN for use in the subsequent step.

-   Go to the AWS Management Console and select DynamoDB under Databases.
-   Click on "Create table" and enter "Rides" as the Table name (case sensitive).
-   Set "RideId" as the Partition key with the key type as String (case sensitive).
-   Enable the "Use default settings" option and click on Create.
-   Wait for the table creation to complete, then select the table name from the Tables page.
-   In the Overview section, click on "Additional info" at the bottom, and note the ARN for future use.

**Create an IAM Role for the Lambda function:** To create the necessary IAM role for the Lambda function, go to the IAM console and select "Roles," then create a new role named "WildRydesLambda" with the role type set to AWS Lambda. Attach the managed policy "AWSLambdaBasicExecutionRole" to grant CloudWatch Logs permissions and create a custom inline policy allowing the "ddb: PutItem" action for your previously created DynamoDB table.

-   Go to the AWS Management Console, select IAM in the "Security, Identity & Compliance" section, and choose "Roles" in the left navigation pane.
-   Create a new role named "WildRydesLambda" with the role type set as AWS Lambda, attach the "AWSLambdaBasicExecutionRole" managed policy, and create a custom inline policy named "DynamoDBWriteAccess" allowing the "PutItem" action for the previously created DynamoDB table.

![A screenshot of a computer Description automatically generated with medium confidence](media/a570d697d6e85327556a4fc747bd32c4.png)

**Create a Lambda function for handling request:** To create the core function for processing API requests and dispatching unicorns, go to the AWS Lambda console and create a new Lambda function named "RequestUnicorn." Copy and paste the code from the provided example implementation (requestUnicorn.js) into the Lambda console's editor and ensure that you configure the function to use the previously created "WildRydesLambda" IAM role.

Access the AWS Management Console and select "Lambda" under the Compute section.

Click on "Create function," choose the "Author from scratch" option, and provide the name "RequestUnicorn" for the function.

Select the "Node.js 16.x" runtime and choose the existing role "WildRydesLambda" from the dropdown.

Scroll down to the "Code source" section, replace the existing code in the code editor with the contents of "requestUnicorn.js," and click on "Deploy."

![A screenshot of a computer Description automatically generated with medium confidence](media/17d4c63267208ee092af4b0de6464b51.png)

**Validate the implementation:** In this module, we will test the function that is created using the AWS Lambda console, and in the next module, we will integrate a REST API with API Gateway to invoke the function from our browser-based application.

-   In the AWS Lambda console, go to the main edit screen of your function and click on "Test."
-   Choose "Configure test event" from the dropdown and keep "Create new event" selected.
-   Enter "TestRequestEvent" as the Event name and copy and paste the provided test event into the editor.
-   ![A screenshot of a computer program Description automatically generated with low confidence](media/3d428d69ef0a10aa9b3270c308dfee44.png)After saving the test event, click on "Test" with the "TestRequestEvent" selected in the dropdown.
-   Scroll to the top of the page and expand the "Details" section of the "Execution result" to verify the successful execution of the function with the expected function result.

![A picture containing text, font, screenshot, line Description automatically generated](media/f9e9b42ae9688a6a92494c9aa8c1293a.png)

**Module 4: Build a RESTful API**

In this module, we will use Amazon API Gateway to expose the Lambda function you built in the previous module as a RESTful API. This API will be accessible on the public Internet. It will be secured using the Amazon Cognito user pool you created in the previous module. Using this configuration, we will then turn our statically hosted website into a dynamic web application by adding client-side JavaScript that makes AJAX calls to the exposed APIs.

**Implementation**

**Create a new REST API:** In the AWS Management Console, follow these steps to create a new API in API Gateway:

-   Click Services and select API Gateway under Application Services.
-   Choose Create API and ensure that New API is selected in the Create new API section.
-   Select Build under REST API and enter "WildRydes" as the API Name.
-   Choose "Edge optimized" in the Endpoint Type dropdown (best for public services accessed from the Internet).
-   Finally, click Create API.

    ![A screenshot of a computer Description automatically generated with low confidence](media/dc2697a94a9504f91e45e6ca2902c1a6.png)

    **Create a new resource and method:** Create /ride resource, add POST method, configure Lambda proxy integration with RequestUnicorn function.

    Here are the steps to create the /ride resource, add a POST method, and configure it with a Lambda proxy integration using the RequestUnicorn function:

-   Click on Resources in the left navigation menu of your WildRydes API.
-   Select Create Resource from the Actions dropdown.
-   Enter "ride" as the Resource Name and ensure the Resource Path is set to "ride".
-   Enable API Gateway CORS for the resource.
-   Click Create Resource.
-   With the newly created /ride resource selected, choose Create Method from the Actions dropdown.
-   Select POST from the dropdown and click the checkmark.
-   Choose Lambda Function for the integration type.
-   Enable the "Use Lambda Proxy integration" checkbox.
-   Select the appropriate Lambda Region.
-   Enter the name of the RequestUnicorn function in the Lambda Function field.
-   Save the configuration. If you encounter an error, verify that the selected region matches the one used in the previous module.
-   When prompted to grant Amazon API Gateway permission to invoke your function, select OK.
-   Go to the Method Request card and click on the pencil icon next to Authorization.
-   Select the WildRydes Cognito user pool authorizer from the dropdown list and click the checkmark icon.

    ![A screenshot of a computer Description automatically generated](media/d6af4cfea9c3f284809116b82fb73b28.png)

    **Deploy the API:** To deploy the API follow the below steps.

-   Click on the Actions dropdown and select Deploy API.
-   Choose [New Stage] from the Deployment stage dropdown.
-   Enter "prod" as the Stage Name.
-   Click Deploy.
-   Take note of the Invoke URL, as you will need it in the next section.

![A screenshot of a computer Description automatically generated with medium confidence](media/ce3c3bd7de928df6f1f40a35b093e29b.png)

**Update the config.js file:** To update the /js/config.js file:

-   Open the config.js file in a text editor.
-   Locate the "invokeUrl" setting under the "api" key.
-   Update the value of "invokeUrl" with the Invoke URL obtained from the top of the stage editor page in the Amazon API Gateway console, corresponding to the deployment stage you created earlier.
-   Ensure that the updated config file still contains the previous module's updates for your Cognito user pool.

    Here is the updated config.js file.

    ![A computer code on a white background Description automatically generated with low confidence](media/84a18d2f7ba4c0de08857443fbf11969.png)

-   Save the modified file and push it to your Git repository to have it automatically deploy to Amplify Console.
-   **Validate the implementation**: Clear your browser cache before proceeding, as there might be a delay in seeing the updated content of the conf ig.js file in your S3 bucket.
-   Visit /ride.html under your website domain.
-   If redirected to the ArcGIS sign-in page, sign in using the previously created user credentials.
-   Once the map loads, click on the map to set a pickup location.
-   Choose "Request Unicorn" and observe the right sidebar notification indicating a unicorn is on its way, followed by a flying unicorn icon moving towards your pickup location.

    ![A map of a city Description automatically generated with medium confidence](media/a623c219f4f200e9f528cbae38907aa7.png)

    **Congratulations! We have built a serverless web application using AWS.**
