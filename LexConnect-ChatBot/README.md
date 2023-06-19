# AWS Lex-Connect PersonalBanker Bot

**Project Overview:** In this project, we will create a customer service chatbot for a fictional FinTech company using **Amazon Lex**. We will then integrate the chatbot with **Amazon Connect** to enable a call centre workflow. It's important to complete the first part of the project before moving on to the second part, as the second part relies on the completion of the first. By finishing the first part, we will establish the foundation of the chatbot, which will then be integrated with Amazon Connect in the second part to create a complete call centre setup. By completing both parts of the project, we will develop a reliable customer service solution that offers seamless communication through both chat and voice interactions.

**Prerequisites:** To finish this workshop, we'll need an AWS Account with the necessary permissions to create AWS IAM, Amazon Lex, Amazon Connect, and AWS Lambda resources.

# Building Chatbots with Amazon Lex

Amazon Lex is a service that allows us to build conversational interfaces in applications using voice and text. It uses advanced deep learning technology for speech recognition and natural language understanding to create engaging and lifelike chatbot experiences. With Amazon Lex, developers can easily build sophisticated chatbots using the same technology behind Amazon Alexa. In this project, we will do the following steps:

-   Creating a Lex bot
-   Adding intents
-   Adding slot types
-   Using AWS Lambda as the back-end logic for Lex

# STEP 1- Creating a Bot

-   Log in to the [AWS console](https://console.aws.amazon.com/lex/home) and navigate to the Amazon Lex service.
-   **Please ensure you have selected North Virginia as the region in the top right (Amazon Connect is not available in all regions yet)**
-   If you have never created a bot, click "Get Started”.
-   Choose "Custom bot", which will display a dialog asking you to define your bot.
-   Our bot name will be "PersonalBanker"
-   Choose your preferred output voice.
-   Session timeout should be 5 minutes.
-   Choose "No" to the Children's Online Privacy Protection Act (COPPA) question.
-   The form should now look as follows, noting that we're going to accept the default IAM role.

   ![p1](https://github.com/rupar19/AWS_Projects/assets/25507934/88321370-35b9-4905-be75-c4bfca604635)


-   Click "Create"
-   We will start by creating an intent, which represents an action that the user wants to perform. For example, we're going to create one intent in this lab for "Get Balance Check”; (If you have time afterwards you can create a separate intent for getting last transaction) Click the "Create Intent" button.
-   In the window that pops-up click the "Create intent" link

   ![p2](https://github.com/rupar19/AWS_Projects/assets/25507934/ecb4f0e5-fdca-4d67-bbdf-a343d97805dc)

-   Our first intent enables the user to get account details, so name this intent "GetBalanceCheck" then click "Add".

    ![p3](https://github.com/rupar19/AWS_Projects/assets/25507934/19c916bc-a642-463e-a747-6c955c89ce52)
    

-   We now want to provide samples of what our user would type or say to perform this action (i.e. to activate this intent). Under "Sample utterances", type the below phrases and hit [enter] or click the blue "+" sign after each phrase. Make sure you do not add a question mark at the end of the phrase as this will cause build issues later.
-   Check my bank balance.
-   how much money is in my account.
-   how much money do I have

    ![p4](https://github.com/rupar19/AWS_Projects/assets/25507934/01a90956-da05-48d4-873e-c3bbbef6351b)

-   Next, we define a slot which is information we need to process the users request. This information can be included in the utterance (query) that the user types or says, and if not included, Lex will prompt the user for the information. While Lex includes many built-in slot types (such as number, colour, city, food, etc), in this case we want to define a custom slot to get the account type that the user is referring to.
-   Click on the blue "+" sign next to "Slot types" on the left-hand side of the screen and select the "Create slot type" link - note, "Slot types" is initially greyed out, and on some laptop screens may not be obvious.

![p11](https://github.com/rupar19/AWS_Projects/assets/25507934/a3624a45-0ad4-4457-8f86-ee09e959c23a)

![p5](https://github.com/rupar19/AWS_Projects/assets/25507934/7a32c75c-0c15-465a-a459-16470ccdbfe3)

-   Click create slot type Add slot type.
-   For 'Slot type name' enter "AccountType" and optionally enter a description (although description is not required)
-   Select restrict to Slot values and Synonyms.
-   For Value, we want to allow the user to make queries against either their "Saving" or "Current" account so enter those as values, clicking the blue "+" sign after each word.

![p6](https://github.com/rupar19/AWS_Projects/assets/25507934/01e8794e-074c-4d6a-abe9-ee01677f819d)

-   Click "Add slot to intent”.
-   We now have to link the slot types to the Intent with additional information such as whether it is required or not and the prompt to use (ie how Lex will ask?) Enter 'What type of account do you have (current or saving?)'.
-   In the existing Slot list change the "Name" field from "slotOne" to "AccountType" so that it matches the slot name that we specified when we created the sample utterances.
-   Specify "What type of account do you want to check (Current or Savings)?" for the "Prompt" field. This prompt will be used by our bot if the user does not specify an account type when asking a question.

   ![p7](https://github.com/rupar19/AWS_Projects/assets/25507934/d16c5958-95bf-48f6-b8aa-05547eec669c)

-   We are now going to ask a security follow up question and ask the user to enter their four-digit pin number.

Add another slot and add the name as "PinNumber". Select the slot type AMAZON.FOUR_DIGIT_NUMBER and add the prompt as "what is your pin number for your {AccountType} account". Ensure you click on the plus icon to add your new slot.

![p8](https://github.com/rupar19/AWS_Projects/assets/25507934/937aa033-91c1-4153-9cef-436a23a9bf41)

It is worth noting as you build other intents you can modify the order of the Slot collection (i.e. what questions get asked in which order) and also whether or not the slot is "Required" before passing the values to our external function.

-   Scroll down and click "Save Intent”.

If at any point you made a mistake in the steps above, selecting the "Latest" version of the intent at the top, next to the intent name, will allow you to edit your choices.

-   Let's build this simple Bot: Hit the grey Build button at the top right corner. You will be asked for confirmation to build. Click "Build".

The build process takes approximately a minute. Once complete, you can ask your bot a question to test it. For example, you could type "what is my balance?" in the chat window, or click the microphone symbol, speak your request and client it again to have Lex translate your speech to text. At this stage since we have not added in the backend Lambda function, the response will be that the bot is ready for fulfilment and will show you the values which will be transferred.

![p9](https://github.com/rupar19/AWS_Projects/assets/25507934/34c2f254-bc97-4e77-a806-fa9a1052b571)

-   It is possible to give the user a simpler interface on the bot to multiple choice questions using Response Cards. If you click on the small cog icon next to the "AccountType" slot you get the option to add a "Prompt response card". Add a title "Select your card type" and add button title Saving Account (choose value Saving) and Current Account (value Current). Click "Save" and rebuild and test. You will now be presented with a multiple-choice option select.

  ![p10](https://github.com/rupar19/AWS_Projects/assets/25507934/959bf0f0-67b8-47c3-ac83-c286e01f5561)

# STEP 2- Fulfilling the Bot

Now we can transfer the slot answers to a function to handle logic and provide a result to the user. We will create a Lambda function with JavaScript code that detects the intent name ('GetBalanceCheck') and returns values based on the AccountType and the accuracy of the entered Pin number. In this Lambda function, we have used a hardcoded Array for data, but in a real-world scenario, we would authenticate the user and perform a database look Use the AWS Console to navigate to Lambda.

-   Click on the orange 'Create a function' link under the 'Getting Started' section.
-   On the "Create function" page, click the "Author from scratch" button.
-   Let's give our function the name of "myPersonalBanker" and optionally provide a description.
-   Choose Node.js 14.x as the Runtime
-   We will "Create new role from template – give it a Lex-style role name (such as "LexRole") and select "Test Harness permissions" as the policy template. Up to retrieve the actual account balances.

    ![p12](https://github.com/rupar19/AWS_Projects/assets/25507934/9e3036eb-5a5f-475e-afcd-9f4b46d97cad)

-   Click "Create function" on the bottom right and you'll be take to the "Configuration" window. We are not adding any additional triggers, nor are we using Lambda Layers, so scroll down to the "Function code" section.
-   Open the lambda function code you will find here (myPersonalBanker_v1.js). Copy and paste the code into the inline editor – make sure that you overwrite any template code that is already in the code box.
-   Scroll down to the '"Execution role" section and ensure that the role you created previously is selected in the "Existing role" drop-down – if not then please select it
-   Leave the rest unchanged, then hit the orange "Save" button at the top of the screen.

# Step 3: Link the bot with the Lambda function

In this step, we will connect the three intents we created to the Lambda function. By specifying the Lambda function as the method containing the business logic to fulfil user requests, when a user indicates an intent (e.g., "What is my checking account balance?"), Lex will invoke the Lambda function, passing the intent name ("GetAccountDetail") and the slot value ("checking").

To do this, we go back to the Lex Console.

-   Click on Personal Banker
-   Ensure the 'GetBalanceCheck' intent is selected.
-   Make sure that the 'Latest' version is selected for both the bot and the intent.

  ![Picture13](https://github.com/rupar19/AWS_Projects/assets/25507934/17d26f3e-8682-4c31-935b-6e44539d8134)

-   Scroll down to "Fulfillment", select "AWS Lambda function", choose "myPersonalBanker" and click "OK" in the popup warning window which opens. It indicates you are giving Lex the permission to run this Lambda function.

![Picture14](https://github.com/rupar19/AWS_Projects/assets/25507934/c75cf6fd-4ca7-43f2-8f52-c7980e195bc6)


-   Click "Save intent"
-   Repeat the above steps 3, 4 and 5 for intents "GetLoanDetail" and "GetLoanProducts”.
-   Click "Build" and then click "Build" again on the confirmation screen.

# Step 4: Running and debugging the Bot

-   After testing the bot as we did in Step 1, we will receive a sample response from the Lambda function. The current function is designed to demonstrate a basic flow, and in the upcoming steps, we will enhance the code to make it more helpful.
-   The existing JavaScript code in the "simpleResponse" function simply returns data to the user without much processing. To make the application more useful, our goal is to pass values into the function and retrieve data from a data source, such as an array or a database in a production environment.

To modify the Lambda function, we need to locate the line containing "return simpleResponse (intentRequest, callback);" and comment it out by adding "//" at the beginning. Then, we remove the "//" from the line below it, so it appears as:

![Picture15](https://github.com/rupar19/AWS_Projects/assets/25507934/4956a22f-7b40-4105-a16f-65e2de58cc17)

Save the lambda function and retry the Lex bot. First of all, try by choosing 'Saving' and saying the 'PinNumber' is 1234. We should now get a nice response from lex telling you of your account balance. However, now your Lex box will error if you enter an incorrect PinNumber which is not helpful to the user.

![Picture16](https://github.com/rupar19/AWS_Projects/assets/25507934/54fa4a37-2f11-417e-b662-931976c332da)


-   Finally, we are going to add some error handling and a feedback loop to the user until a correct PinNumber is entered. The code will check to see if there is an account match and if not will request the user tries again and resets the 'Slot'.

    If we modify the lambda function and look for the line 'return balanceIntentError(intentRequest, callback);' and place '//' before this line and remove the '//' from the line below so it looks like:

  ![Picture17](https://github.com/rupar19/AWS_Projects/assets/25507934/c3dec7f4-e45f-4105-818e-efa3fdfc5bbe)

 ![Picture18](https://github.com/rupar19/AWS_Projects/assets/25507934/dcdd1e14-53c6-425a-81ed-54a27b6bb234)


# Conclusion

In this project, we have learned the fundamental steps to manage a Lex bot, including creating a bot, defining intents and slot types, and connecting a Lambda function to the chatbot, with the continuation in the second part of the project, so please refrain from deleting the Lex Bot created in this exercise.

# Building a Call Centre with Amazon Connect

Amazon Connect is a cloud-based contact centre solution that enables us to quickly set up and manage customer contact centres, providing seamless customer engagement at any scale, including easy agent onboarding and interaction with customers. In this project, we will do the following steps:

-   Creating a Call Centre
-   Create a simple contact flow.
-   Adding a Lex Bot to your flow
-   Improving your Call Flows
-   Extra: Getting personal with your caller

# Step 1: Setting Up a Call Centre

-   Log in to the AWS console and navigate to the Amazon Connect service.
-   Please ensure you have selected North Virginia as the region in the top right (Amazon Connect is not available in all regions yet)
-   If you have never created a Call Centre, click "Get Started”.
-   Enter a custom Access URL, then click 'Next Step’.
-   For this lab we will be using the default Admin account - so you can choose to "Skip" the Administrator account. Then click 'Next Step’.
-   For this lab you will not need to make Outbound calls, uncheck this option and click 'Next Step’.
-   The Connect application will automatically create you a S3 bucket and encrypt the data. Click 'Next Step'
-   Finally click 'Create Instance'.

    ![A screenshot of a computer Description automatically generated with medium confidence](media/c1cde01afda7d8066e7e38dd11cda43e.png)

-   This may take a minute or two to setup.
-   Once it is setup you will be presented with a screen which allows you to 'Get Started'
-   The browser will pop up a warning to ask you show notifications - this is to allow the Connect Application to notify you when a call is coming in. Click allow on the pop up.

    ![](media/01ae574a3c7f8274c62e10636c818a06.png)

    ![A picture containing text, screenshot, font Description automatically generated](media/3f76aa1eadc9d9ee8c849289c74d5613.png)

-   At this point your Call Centre is setup but you don't have any numbers, so you need to 'Claim your first number' - For this lab chose a UK Direct Dial Number and click 'Next'.
-   This is the first number for your call centre - you now can call the number and go through the default first call example that is part of Amazon Connect by ringing the number that is allocated to you or click 'Skip for now'.

# Step 2: Create a simple Contact Flow

Amazon Connect revolves around creating customizable contact flows, and in this step, we will create a basic flow to showcase how to set up a call, which includes a customer welcome message and placing them in the agent queue.

When creating a new Connect environment, there is a default workflow available under the 'Routing' \> 'Contact Flows' section called 'Sample inbound flow (first call experience)'. However, for simplicity, we will create our own customized 'Simple flow'.

![](media/40c9a44d55b68fcc3ed3c2a713860e01.png)

-   We are going to import a premade 'Simple contact flow'. First of all download the following file to your computer Simple Workflow.
-   Head back to the 'Contact Flows' section listed above and in the top right click on 'Create contact flow'.
-   In the top right click on the down arrow next to the "Save" button and click Import Flow (beta).

![](media/7943f210bb8c142e1d3b3226ab457475.png)

-   Browse and find the file labelled Simple Workflow
-   Once the flow is uploaded you will see the following flow:

![](media/b14f44271741a1387ac9556aaaa0dcc4.png)

The top line of the flow diagram configures the flow, including the choice of Amazon voice for dialogue prompts, enabling call recordings to be stored in S3 buckets, and routing all calls to the default queue.

On the second line we create a message to say to the customer dialing in:

![](media/fd038b89c7714d35d243abd7f4d35e0d.png)

This is completely customisable, and we can even record our own audio prompt. Following this we transfer the caller to the queue ready for an advisor.

The other items in the flow relate to what happen when an error occurs and the end of the call.

-   The Contact Flow is complete, and we need to Publish it by clicking on the arrow next to save and choosing 'Save & Publish’.

![](media/5743c0dc7afe36f3ceace1157a9b4d73.png)

-   The flow is now created but it is not linked to the telephone number we created earlier. To associate the flow - chose 'Phone Numbers' from the Routing menu

![](media/3725ed92717574c7fcf8afbcaa0bdebc.png)

Click on the number and you will be presented with an Edit screen - choose Simple Workflow in the Contact flow / IVR section and click Save.

![](media/17793d1e48a5b4ff5ffc650c8912e74c.png)

-   Wait a few seconds and then call your phone number again and see how the flow has changed. Maybe modify the text in the Play Prompt section of the flow and Republish and call in again and see how it changes?
-   If you want to act as a Call Agent, you need to click on the phone icon in the top right of the screen and ensure you are set to 'Available’.

    ![](media/c4b2a264d7afe5aee1138253127df968.png)

# Step 3: Adding your Lex bot into the flow.

We are now going to leverage the Lex Bot within the contact flow.

-   First of all we need to give Amazon Connect permission to access your lex bot. Head to AWS Console page for your connect app and chose your Connect instance.
-   Click on Contact flows and in the Amazon Lex section you can chose the Lex Bot you created earlier (ensure you click + Add Lex Bot link)

    ![](media/ccf7d2b9415cc615dbce51698950b1c3.png)

    We are now going to modify the simple workflow to integrate your Lex bot. On the left-hand menu chose "Interact" and drag "Get Customer Input" into your workflow.

    ![](media/c3d4e05a95a74df7652d82714a887e72.png)

    If we click on the top line of the box, you have dragged in you will be presented with the following option on the right-hand side of your screen:

    ![Phone numbers edit](media/63f57ccfe4868ded49a1681492d92f82.png)

Within this edit window we are going to add a starting question (to introduce the bot) - click on 'Text to speech' and enter some text into the box below (eg "How can we help you today?").

Chose Amazon Lex and select your Lex Bot from the drop-down list.

Finally, we need to add the "Intents" you wish to allow flows from - add the intent "GetBalanceCheck" (this is the Intent we created in previous steps while setting up the chat Bot).

-   We now need to adjust the Simple workflow to introduce our Lex bot. First, you need to remove the link between the Play Prompt and Transfer to queue. You can remove a link by hovering over the arrow and clicking on the red cross.

![](media/2f17b7049da00d1cb813fd24d89ecf93.png)

-   Once the link has been removed, we need to introduce our recently created Input. Click and hold on the circle on the Okay within Play prompt and join it to our new box. When a connection is successfully made there will be a yellow circle at the end of the block

    ![](media/09361b8e648464c9c058c009e825d5aa.png)

    We now have the input flow, but we need to connect the output of the Input. If you join the "Default" option to the "Transfer to Queue" block and the other two options to "Disconnect / Hang Up".

    At this point click 'Save & Publish' and then try redialling your number you will be presented with a new option. If you respond to the question "How can we help you today?" with the phrase "I'd like to check my balance" you will now enter your Lex bot flow that we have built previously.

# Step 4: Improving your Call flows.

In the above example after introducing the Lex bot our customer is now actually unable to speak to an advisor as they can only get the balance of their account.

To create a flow back to an advisor we need to go back to edit the Lex bot we created in Lab 1 and create a new Intent that listens for "Speak to an advisor”.

-   Head back to the Lex bot you created in Lab 1 AWS console. Click on the + symbol next to Intents and add a new intent called "Advisor”.

    ![](media/c38df10f802cb08e5ef7f0a8a528db3f.png)

-   Add the following utterances:
-   advisor
-   talk
-   talk to someone.
-   talk to advisor.

    These are all phrases which the user might say to invoke the Call Centre Advisor (feel free to add any others you can think of).

-   We are also going to add the utterance:
-   one

    Adding this utterance will allow the customer to engage with our Lex Bot via the keypad as well (i.e. we can tell the customer to say "Speak to Advisor or Press 1 on the keypad")

    our utterances should now look like this:

    ![](media/fe3ac80fcaedbab997b0239f6f4904da.png)

-   Click 'Build' at the top.
-   At this point we should also modify the GetBalanceCheck Intent to also add the utterance:
-   Two

for the same reason as above.

-   Now we need to add the intent to our Connect flow. Head to your flow and click on Get Customer Input Flow. Click on "Add another intent" and type Advisor.

![](media/8b73e42040d284dc00cd09ee4e5edd1a.png)

-   Having added an additional Intent this is now added to the flow.

![](media/0cc48d4a6580779c30bfe4add6be394a.png)

-   You will want to join the arrow from the "Advisor" intent into the transfer to queue flow option.

    ![](media/1d288ed4b495eb785aad88c2616e3784.png)

-   Click Save & publish and recall your contact centre. You can now either ask for an advisor (or press 1) on your keypad or continue with the Lex flow you built earlier by selecting 2.

# Step 5: Interacting with your user based on their dialer number (Extra Credits)

When customer calls Amazon Connect the service has access to the caller number so it is possible to build functionality to personally greet the user when they dial in using Lambda. This section is slightly less guided as it extends on a lot of the topics already covered.

-   Create a new Lambda function named "myPersonalResponder"
-   Open the lambda function code you will find here (myPersonalResponder_v1.js). Copy and paste the code into the inline editor – make sure that you overwrite any template code that is already in the code box and save. At the top of the file is a
-   Within the Connect Admin (where you added your Lex bot) you can also attach Lambda functions. You will need to modify your workflow to add your new Lambda.

    ![](media/86d93f3072761b2d1f329e8cefee1910.png)

-   Head to your workflow and Click Integrate from the left-hand menu and drag "Invoke AWS function

    ![](media/9422c3b3ce1f54bdd3c2cb37b5bb4f37.png)

    " And add the lambda function to the workflow:

    ![](media/6795b375af8a41ea55173f3cea03ca47.png)

-   Review the Advanced_Workflow by Importing it as a new Flow, in this flow we add following steps:
-   Invoke our AWS Lambda function
-   The Lambda function returns an Object in which we define whether there is a CustomerMatch (CustomerMatch = 0/1)
-   If there is a Customer Match:
-   We branch off and Set a Contract Attribute to equal the Customer Name returned from the Lambda function
-   We play a different prompt to the user which is personalised and uses the variable we return
-   If there is No customer match, we simply respond with the original welcome message.
-   Replicate this flow in the Simple Workflow you have been using during this lab to add this functionality.

# Conclusion

So now we have successfully built the chatbot and integrated it with Amazon Connect to create a call centre workflow.

# Cleanup

To clean up all the resources created in this workshop:

-   If have created an Amazon Connect instance, go to Amazon Connect console, select the instance, and click Remove
-   Delete the Amazon Lex bot in the Amazon Lex Console
