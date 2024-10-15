# Build your own virtual assistant using genarative AI

<!-- TOC start -->

- [1. Set up Watson Discovery and database](#1-set-up-watson-discovery-and-database)
  - [1.1. Creating a Watson Discovery project](#11-creating-a-watson-discovery-project)
  - [1.2. Create Watson Discovery Collection](#12-create-watson-discovery-collection)
    - [1.2.1. Create a web crawling collection](#121-create-a-web-crawling-collection)
    - [1.2.3. Use ready-made collection](#123-use-ready-made-collection)
  - [1.3. Getting the project ID of Watson Discovery](#13-getting-the-project-id-of-watson-discovery)
- [2. Watsonx.ai configuration](#2-watsonxai-configuration)
  - [2.1. Credentials to Watsonx.ai](#21-credentials-to-watsonxai)
- [3. Setting up the Watsonx Assistant](#3-setting-up-the-watsonx-assistant)
  - [3.1. Creating the a chatbot](#31-creating-the-a-chatbot)
  - [3.2. Setting up the extension for Watsonx.ai](#32-setting-up-the-extension-for-watsonxai)
    - [3.2.1. Add Watsonx.ai extension](#321-add-watsonxai-extension)
    - [3.2.2. Add Watson Discovery extension](#322-add-watson-discovery-extension)
  - [3.4. Set up actions](#34-set-up-actions)
    - [3.4.1 Set up pre-defined actions](#341-set-up-pre-defined-actions)
  - [3.5. Getting the assistant to answer questions](#35-getting-the-assistant-to-answer-questions)
- [4. Test the chatbot](#4-test-the-chatbot)
- [5. Optional: prompting in Watsonx Assistant](#5-optional-prompting-in-watsonx-assistant)
- [6. Finish](#6-finish)

<!-- TOC end -->

In this lab we will go over how to set up a virtual assistant (or chatbot) for customers using IBM Watson services: Watsonx.ai, Watsonx Assistant, and Watson Discovery.

We created a Retrieval Augmented Generation (RAG) pattern to answer user questions with Large Language Models (LLM) using the existing documentation which resides inside the knowledge base.

Functionality performed by the three Watson components:

- Watson Discovery - The knowledge base that contains all the data (web crawled, PDFs, etc.) which the chatbot will base it's answers on.

- Watsonx Assistant - The conversation service through which the user interacts with the chatbot.

- Watsonx.ai - The service that will call the LLM, enabling virtual assistant to generate responses based on user questions.

## 1. Set up Watson Discovery and database

In this chapter we will go through how to set up the Watson Discovery and create a database from different sources.

## 1.1. Creating a Watson Discovery project

Before we can config the database we need to create a project in Watson Discovery where we will store the data, to do so follow these steps:

1. Open the resource list

   ![Watson Discovery](./images/resource-list.png)

2. Click on Watson Discovery in the resource list

   ![Watson Dicovery](./images/open-wxD.png)

3. Click "Launch Watson Discovery".

   ![Watson Dicovery](./images/launch-discovery.png)
   **_Note_**: The prefix of the resource name might be different than on the image.

   **_Note_**: We will use the credentials from Watson Discovery later so save the <span style="color:red"> **API key**</span> and <span style="color:red"> **URL** </span>in a document.
   <!--
   If you have it right it should be the same like this:-->

   <!--
   WD API key: <span style="color:red"> 52jkl5v-B5lOhMI6bDYd0oEq7OYBWgQtWEmfiin7TeqU </span> <br>
   WD URL: <span style="color:red"> api.eu-de.discovery.watson.cloud.ibm.com/instances/5a1487d3-472a-4c4b-a3cc-8684157389e4 </span>
   -->

4. Click on the **New Project** button

   ![Watson Dicovery](./images/Discovery_home_screen.png)

5. Give it a **name with your initial** so you can always find your project. We are conducting the lab in a shared Watson Discovery instance and you may see others project. Make sure you always do changes in your project.

   ![Watson Dicovery](./images/Discovery_create_project.png)

6. Select **document retrieval** and click next

### 1.2. Create Watson Discovery Collection

Now that you have created a project we need to create a collection where you can store your data. A collection is like a folder where you can store different data from different data sources in multiple collections inside one project.

<!--
Now you have options:

- Option 1: Create a collection from PDFs - [Section 1.2.1](#121-create-a-collection-from-pdfs)
- Option 1: Create a collection from webcrawling [Section 1.2.2](#122-create-a-web-crawling-collection)
- Option 3: Use ready-made collection already crawled on the webpage [AI Sweden](https://www.ai.se/en) - [Section 1.2.3](#123-use-ready-made-collection)

Choose **ONE** of the options and follow the instruction. Once you are done you can continue in next chapter. -->

<!-- #### 1.2.1. Create a collection from PDFs

![Watson Dicovery](./images/Discovery_create_collection_1.png)

1. Give this collection a name
2. Choose the language used in the document
3. Upload the file. You can use our prepared PDF document in the "Assets" folder, the PDF is called: **aivision_eng-1**

4. Click finish -->

#### 1.2.1. Create a web crawling collection

1. Scroll down the page and click the **here** text to connect to a data source

   ![Watson Dicovery](./images/Discovery_create_collection_2.png)

2. Choose the language of the data you want to crawl

   ![Watson Dicovery](./images/Discovery_web_crawler1.png)

3. Scroll down in the page and add the URL of the website that you would like to crawl, you can add multiple sections of a website. If you're unsure, call on the lab assistant. Press add when you're done.

   ![Watson Dicovery](./images/Discovery_web_crawler2.png)

4. Click finish

<!--
#### 1.2.3. Use ready-made collection

A pre-made web-craweled collection is in another project.

1. Go to **My projects**
   ![Discovery_my_projects](./images/Discovery_my_projects.png)

2. Choose project **Put AI to Work - AI Sweden**
   ![AI Sweden project](./images/Discovery_putaitowork_project.png)
 -->

Watson Discovery will now start to prepare your data and it can takes some time. Meanwhile we can continue to set up other services.

### 1.3. Getting the project ID of Watson Discovery

1. On the left menu bar click on **Integrate and deploy**

   ![Discovery_integration_and_deploy](./images/Discovery_integration_and_deploy.png)

2. Click on **API Information**

   ![Watson Dicovery](./images/Discovery_projectid1.png)

3. Save the <span style="color:red"> **Project ID**</span> in a document and mark with **WD-Project-ID**, we will use it later to integrate with Watsonx Assistant.

## 2. Watsonx.ai configuration

<!--
1. Go back to the resource list and open Watson Studio.
   ![Watson Studio](./images/open-watson-studio.png)

2. Launch in IBM watsonx.
![Watson Studio](./images/launch-in-wx.png)


### 2.1. Create Watsonx.ai project

3. Scroll down the page and click on the "+" icon to create a watsonx project.
   ![Watson Studio](./images/create-wx-project.png)

4. Give your project a name with your initials so you can always find it.
   ![Watson Studio](./images/create-wx-project-2.png)

5. Go to **Manage -> General** Tab and copy the <span style="color:red"> **project ID**</span> to your notes and mark it with **WX-project-ID**.
   ![Watson Studio](./images/get-wx-project-id.png)

### 2.2. Add associated service

Go to **Manage -> Services & integrations**, and add associate service.

![Watson Studio](./images/add-wx-service.png)

Choose **Watson Machine Learning** and click on **Associate**.

![Watson Studio](./images/add-wx-service-wml.png)

### 2.3. Credentials to Watsonx.ai

1. Click on the **IBM watsonx** on the top-left to nevigate back to home and click on **Access (IAM)**.
   ![Watson Studio](./images/create-IAM.png)

2. Navigate to **API keys** and click on **Create**
   ![Watson Studio](./images/create-IAM-2.png)

3. Give it a name and click create. Then copy the <span style="color:red">API key </span> to your notes and mark it as **WX-API-KEY**. -->

### 2.1. Credentials to Watsonx.ai

Watsonx.ai is the AI platform facillitate access to LLM. We have created a watsonx.ai project for you as well as the associated API key to the service.

Save the following credentials into your notepad. They will be used in the next steps where we integrate watsonx.ai the the chatbot.

> WX-API-key: <span style="color:red"> GpId1X3uWWyZJYBDhkoT5u83heCDKt23Kx8sFMf0evKj </span> <br>
> WX-project-ID: <span style="color:red"> df9e9ba2-128f-4f5e-a34e-ff5a21894136 </span> <br>

We have all the information we need. We can go to Watson Assistant to start creating the demo.

## 3. Setting up the Watsonx Assistant

In this section we will integrate the other services so that we can ask the assistant questions.

### 3.1. Creating the a chatbot

1. Go back to "IBM Cloud" home page and click on Resource list

   ![Watson Dicovery](./images/resource-list.png)

2. locate Watsonx Assistant in the resource list

   ![Watson Dicovery](./images/watsonxassistant.png)

3. Launch the Watsonx Assistant
   ![WxA launch](./images/Watsonx_assistant_launch.png)

If it is the first time opening the service we need to create a first assistant. Follow step 3 - 5.

3. Give your chatbot a name with your initials and description, keep the Assistant Language in English. Click on next.

   ![Watson Dicovery](./images/Assistant_create.png)

4. Choose Web in the first field. The second field can be whichever industry you prefer. The third field should be **Developer** and the last field should be **I want to provide confident answers to common questions**

   ![Watson Dicovery](./images/Assistant_create2.png)

5. Customize your chatbot and press next.

   ![Watson Dicovery](./images/Assistant_create3.png)

If there already exist some chatbots, follow step 6 - 8 to create your chatbot.

6. Click on the chabot name (this can be different from your view) on the navigate bar on the top to expand the folder. Click on **Create New**.
   ![WxA create new](./images/WxA_create_new_chatbot.png)

7. Give your chatbot a name with your initials and choose the languague for it.
   ![WxA create new](./images/WxA_Create_new_2.png)

### 3.2. Setting up the extension for Watsonx.ai

To build custom integration for your chatbot, we will need to create two extensions in Watsonx Assistant: 1) extension for Watsonx.ai (to connect to an LLM model); 2) extension for Watson Discovery (where our data resides). We will use the credentials gathered and saved earlier to connect Watson Discovery and Watsonx.ai services to Watsonx Assistant. We will perform these three steps in Watsonx Assistant:

- Set up an extension to integrate Watsonx Assistant with Watsonx.ai.

- Set up an extension to integrate Watsonx Assistant with Watson Discovery.

- Create our first Watsonx Assistant actions. We will upload ready-to-use actions that created for this lab.

It does not matter which of the two extensions we add first. In this guide we will begin with Watsonx.ai extension.

#### 3.2.1. Add Watsonx.ai extension

1. In the assistant menu on the left, click on **Integration**. If you can't find the menu, first click on **"IBM watsonx Assistant Plus"** on the top left corner to get back to the Home page before locating the assistant menu.

   ![Watsonx Assistant](./images/Watsonx_assistant1.png)

2. On next page, scroll down until you find **Extensions** and then click on the **Build custom extension** button.

   ![Watsonx Assistant](./images/Watsonx_assistant2.png)

3. You can start by clicking on **Next** when you see the first page.

![Watsonx Assistant](./images/customextension.png)

4. Give a name to your custom extension, for example _Watson Discovery extension_ and optionally add a description to the extension. Click **Next** when done.

![Watsonx Assistant](./images/customextension2.png)

5. Upload the openAPI specification `watsonx-openapi.json` that is found [here](https://ibm.box.com/s/spzf8qwnk7rf05bols8pdsg308jd8hjr) (password: <span style="color:red"> Watsonx2024</span>). Click on **Next**, and then click **Finish**.

6. Click on the **Add** button on the newly created extension and click then **Next**.

   <img src="./images/Watsonx_assistant10.png" alt="drawing" width="200"/>

7. Change the Authentication type to _oAuth 2.0_. Add the <span style="color:red">**WX-API-key**</span> key that you saved from [2.1.](#21-credentials-to-watsonxai) then click **Next**, then **Finish**.

   ![Watsonx Assistant](./images/Watsonx_assistant12.png)

#### 3.2.2. Add Watson Discovery extension

Next thing we will do is create a Watson Discovery extension.

1. Click on **Build custom extension.** again.

2. Name your extension, for example _Watson Discovery extension_ and optionally add a description.

3. Upload the openAPI specification for the Watson Discovery extension `watsondiscovery-query-openapi.json` which you can be find [here](https://ibm.box.com/shared/static/q3mco931ve5zy2l2slf0duxwqhawk5ar.json) (password: <span style="color:red"> Watsonx2024</span>). Click on **Next**, then **Finish**.

4. Click on the **Add** button on the Watson Discovery extension. Click **Next**, then change the authentication type to _Basic auth_.

5. Write "**apiKey**" as the username.

6. Add the **WD API key** that you saved from section [1.1](#11-creating-a-watson-discovery-project) in the password section and in the Server variables add the **WD URL** found in Watson Discoveries launching site.

   **Note:** Remove the **https://** part of the URL in the _Server variables._

   ![Watsonx Assistant](./images/Watsonx_assistant3.png)

   Once you're done, click **Next** and then **Finish**

### 3.4. Set up actions

An action represents a discrete outcome that we want our assistant to be able to accomplish in response to a user's request. An action comprises the interaction between a customer and the assistant about a particular question or request. Actions allow us to define tasks that the assistant can perform, such as calling external services like Watson Discovery and Watsonx.

#### 3.4.1 Set up pre-defined actions

When creating your actions in Watsonx Assistant, you can created them from scratch or import existing actions that someone already created for another assistant. The latter approach is what we call using pre-defined actions. To upload pre-defined actions, do the following:

1. Go back to the home screen of Watsonx Assistant. In the menu on the left, click on the **Actions** menu option. Click the cogwheel logo in the top right corner of the page.

   ![Watsonx Assistant](./images/WxA-prepare-upload.png)

2. Click on the **Upload/Download** tab as shown in image bellow.

![Watsonx Assistant](./images/Watsonx_assistant4.png)

3. Click on the **Upload** button, or drag and drop `Action_AI.json` [here](https://ibm.box.com/s/xkt2yx6brq52c78a7yg3b8ifeh2jjlyx) (password: <span style="color:red"> Watsonx2024</span>) which contains downloaded actions.

4. When the action is uploaded press the **saved** button in the top right corner.

### 3.5. Getting the assistant to answer questions

Once you have actions in your assistant, they will look similar to the picture below.
![Watsonx Assistant](./images/WxA-uploaded-actions.png)

Do the following to config variables:

1. Click on the uploaded action called **Q&A Llama 3 general answer**. Then go to step number 3 as shown in the picture below.

   ![Watsonx Assistant](./images/Assistant_discovery.png)

2. Click on **Edit extension** on the bottom. Go to the third step of this action, _Parameters_ - this is where Watson Discovery is used.

3. Change the project ID to the same we got in section [1.3](#13-getting-the-project-id-of-watson-discovery) (should be **WD-Project-ID**).

   ![Watsonx Assistant](./images/editextension.png)

And then click on **Apply**

4. Go to the step 11 of the action - this is where we call Watsonx.ai.

   ![Watsonx Assistant](./images/Assistant_Watsonx.png)

5. Click "Edit extension" and change the Watsonx.ai project ID to the project ID you got in [2.1.](#21-credentials-to-watsonxai) (named **WX-project-ID**). And then click on **Apply**.

6. Click on the save button which looks the **floppy disk icon** in the top right corner.

   <img src="./images/WxA-save-action.png" alt="drawing" width="200"/>

## 4. Test the chatbot

Whoho! We have successfully created a chatbot.

In menu on the left, click on the **Preview**, then on the right hand side in the assistant window, ask a question.
![Watsonx Assistant](./images/Assistant_test.png)

## 5. Optional: prompting in Watsonx Assistant

If you want to change the prompt for the model used by the chatbot do the following:

1.  Go to the 10th step of the action, then click on the field marked with red square in the picture below.

![Watsonx Assistant](./images/Watsonx_assistant8.png)

2. Inside the text block is the prompt used to improve the performance of the large language model.

   ![Watsonx Assistant](./images/Assistant_prompt.png)

You may want to change the prompt for various reasons such as to get even better results, for other use cases or to support other languages.

## 6. Finish

Thank you for participating in this lab! We hope you enjoyed exploring **Watsonx.ai**, **Watson Assistant**, and **Watson Discovery**, and that you found the process of building a virtual assistant with Retrieval-Augmented Generation (RAG) insightful. These powerful tools are designed to help you push the boundaries of AI-driven solutions.

We encourage you to continue experimenting, innovating, and discovering new ways to integrate AI into your business.

We look forward to seeing the amazing things you will build with our products in the future. Enjoy the rest of your journey, and thank you again for your participation!
