# Robotic Calculations and Inference Agent


This journey takes you through end to end flow of steps in building an interactive interface between NAO Robot, Watson Conversation API & Data Science Experience. Nao Robot listens to the vocal query and converts it into text and sends it to Node Red Flow. The Node Red Flow uses Watson Conversation API to break and map the text into intents and entities for which it has been trained. The conversation API is linked through Node Red Flow to a Jupyter Notebook in Data Science Experience(DSX). In this notebook statistical analysis is performed on the data set. The result of the query is sent back to Node Red Flow through websocket which is sent to Nao Robo and it speaks the result.

When the reader has completed this journey, they will understand how to:

* Establish the communication between Robot & IoT devices.
* Develop a custom web user interface using Node-RED. 
* Create the Watson Conversation chat bot Application.
* Do the statistical analysis on dataset using Jupitor (python) Notebook on Data Sacience Experience.


The intended audience for this journey are developers who want to develop a complete analytics solution on DSX with a custom web user interface. 

![](doc/source/images/Robot_Architecture_Diagram.png)

1. The user asks the specific questions to the Robot.
2. Robot will convert the speech into text and it will send the text to Node-Red Flow for further processing on the cloud. The results from the processing on the cloud is returned to the NAV Robot through the Node-Red flow.
3. Node-RED flow sends the text from NAO robot to the Watson Conversation API. 
4. The Watson Conversation API takes the text input in the form of natural language and breaks and maps it to intents and entities for which it has been trained for.
5. The context of the conversation is saved to the Cloudant DB so that the Conversation API is able to save the state and track the conversation flow of the user.
6. The dataset for analysis is stored in the Object storage.
7. Data is utilized as csv files.
8. The Jupyter notebook receives the Watson Conversation Service API output from Node-RED using Web Sockets. The notebook processes the data based on the question and generates insights. The insights are sent back to the Node-RED flow using Web Socket.
9. The Jupyter notebook is powered by Spark.

## Included components

* [Nao-Robot](https://www.ald.softbankrobotics.com/en/robots/nao/find-out-more-about-nao): The fruit of a unique combination of mechanical engineering and software, NAO is a character made up of a multitude of sensors, motors and software piloted by a made-to-measure operating system: NAOqi OS.

* [Node-RED](https://console.bluemix.net/catalog/starters/node-red-starter): Node-RED is a programming tool for wiring together APIs and online services.

* [Watson-Conversation-API](https://www.ibm.com/watson/services/conversation/?cm_sp=IBMCode-_-create-cognitive-retail-chatbot-_-included_components-_-watson-conversation): Build, test and deploy a bot or virtual agent across mobile devices, messaging platforms, or even on a physical robot.

* [IBM Data Science Experience](https://apsportal.ibm.com/analytics): Analyze data using RStudio, Jupyter, and Python in a configured, collaborative environment that includes IBM value-adds, such as managed Spark.

* [Bluemix Object Storage](https://console.ng.bluemix.net/catalog/services/object-storage/?cm_sp=dw-bluemix-_-code-_-devcenter): A Bluemix service that provides an unstructured cloud data store to build and deliver cost effective apps and services with high reliability and fast speed to market.

* [Jupyter Notebooks](http://jupyter.org/): An open-source web application that allows you to create and share documents that contain live code, equations, visualizations and explanatory text.

## Featured technologies

* [IoT](https://developer.ibm.com/code/technologies/iot/): The inter-networking of large volumes of physical devices, enabling them to collect and exchange data.
* [Data Science](https://developer.ibm.com/code/technologies/data-science/): Systems and scientific methods to analyze structured and unstructured data in order to extract knowledge and insights.
* [Analytics](https://developer.ibm.com/code/technologies/analytics/): Analytics delivers the value of data for the enterprise.
* [Python](https://www.python.org/): Python is a programming language that lets you work more quickly and integrate your systems more effectively.
* [Jupyter Notebooks](http://jupyter.org/): An open-source web application that allows you to create and share documents that contain live code, equations, visualizations and explanatory text.

# Watch the Video

[![](http://img.youtube.com/vi/pic.jpg)](https://youtu.link)

# Steps

Follow these steps to setup and run this developer journey. The steps are
described in detail below.

1. [Sign up for IBM Bluemix](#1-sign-up-for-IBM-Bluemix)
1. [Create Bluemix services](#2-create-bluemix-services)
1. [Configure Watson Conversation Application](#3-configure-watson-conversation-application)
1. [Import the Node-RED flow](#4-import-the-node-red-flow)
1. [Note the websocket URL](#5-note-the-websocket-url)
1. [Sign up for Data Science Experience](#6-sign-up-for-data-science-experience)
1. [Create the notebook](#7-create-the-notebook)
1. [Add the data](#8-add-the-data)
1. [Update the notebook with service credentials](#9-update-the-notebook-with-service-credentials)
1. [Run the notebook](#10-run-the-notebook)
1. [Results sent to the Node Red Flow](#11-results-sent-to-the-node-red-flow)

## 1. Sign up for IBM Bluemix

Sign up for IBM [**Bluemix**](https://console.bluemix.net/). By clicking on create a free account you will get 30 days trial account.

## 2. Create Bluemix services

Create the following Bluemix service by following the link to use the Bluemix UI. Choose an appropriate name for the Node-RED application - `App name:`. Click on `Create`.

  * [**Node-RED Starter**](https://console.bluemix.net/catalog/starters/node-red-starter)
  
  ![](doc/source/images/bluemix_service_nodered.png)
  
  * On the newly created Node-RED application page, Click on `Visit App URL` to launch the Node-RED editor once the application is in `Running` state.
  * On the `Welcome to your new Node-RED instance on IBM Bluemix` screen, Click on `Next`
  * On the `Secure your Node-RED editor` screen, enter a username and password to secure the Node-RED editor and click on `Next`
  * On the `Browse available IBM Bluemix nodes` screen, click on `Next`
  * On the `Finish the install` screen, click on Finish
  * Click on `Go to your Node-RED flow editor`  

  * [**Watson Conversation Service**](https://console.bluemix.net/catalog/services/conversation)

  ![](doc/source/images/bluemix_service_conversation.png)

  * On the newly created Conversation Service page, Click on `Service credentials` then `View credential` and note down the credentials for future use.

  ![](doc/source/images/conversation_credantial_note_down.png)


  * On the same page, on the left side now Click on `Manage` icon then on the right side click on `Launch tool` to launch the Conversation Workspaces. 

## 3. Configure Watson Conversation Application

Launch the **Watson Conversation** tool. Use the **import** icon button on the right

<p align="center">
  <img width="400" height="55" src="doc/source/images/import_conversation_workspace.png">
</p>

Find the local version of [`conversation/workspace.json`](conversation/workspace.json) and select
**Import**. Find the **Workspace ID** by clicking on the context menu of the new
workspace and select **View details**.

<p align="center">
  <img width="350" height="250" src="doc/source/images/open_conversation_details.png">
</p>

after clicking on the View details you will be able to note down the **workspace ID** for future use.

<p align="center">
  <img width="350" height="250" src="doc/source/images/conversation_workspace_id.png">
</p>

*Optionally*, to view the conversation Intents, Entities and Dialog select the workspace and choose the **Intents** tab, **Entities** tab and **Dialog** tab, here it is represented through snippets respectively:

<p align="center">
  <img width="400" height="650" src="doc/source/images/conversation_intents.png" title="Intents">
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <img width="400" height="650" src="doc/source/images/conversation_entities.png" title="Entities">
</p>

![](doc/source/images/conversation_dialog.png)

## 4. Import the Node-RED flow

The flow json for Node-RED can be found under `node-red-flow` directory. 
* Download the `Robotic_AI_Agent_workflow.json`
* Open the file with a text editor and copy the contents to Clipboard
* On the Node-RED flow editor, click the Menu and select Import -> Clipboard and paste the contents

 ![](doc/source/images/import_nodered_flow.png)
 <br/>
 <br/>
 
 #### Configure the Conversation API Credentials
* By double click on the `conversation` node, `Edit conversation node` prompt will open.
* While creating the conversation service we have to note down the credantials for conversation service. The credeitials have to be copied and pasted in the Username, Password and Workspace ID respectively.

![](doc/source/images/conversation_service_credantial_update.png)

 #### Deploy the Node-RED flow by clicking on the `Deploy` button

![](doc/source/images/deploy_nodered_flow.png)

## 5. Note the websocket URL

![](doc/source/images/note_websocket_url.png)

The websocket URL is ws://`<NODERED_BASE_URL>`/ws/Robot_webpage  where the `NODERED_BASE_URL` is the marked portion of the URL in the above image.
### Note:
An example websocket URL for a Node-RED app with name `myApp` - `ws://cognitiverobot.mybluemix.net/ws/Robot_webpage` where `myApp.mybluemix.net` is the NODERED_BASE_URL. 
The NODERED_BASE_URL can have an additional region information say `eu-gb` for UK region and NODERED_BASE_URL could be `myApp.eu-gb.mybluemix.net`. 

## 6. Sign up for Data Science Experience

[**Data Science Experience**](http://datascience.ibm.com/). By signing up for Data Science Experience, two services: ``DSX-Spark`` and ``DSX-ObjectStore`` will be created in your Bluemix account.

## 7. Create the notebook

Open IBM Data Science Experience. Use the menu on the top to select `Projects` and then `Default Project`.
Click on `Add notebooks` (upper right) to create a notebook.

* Select the `From URL` tab.
* Enter a name for the notebook.
* Optionally, enter a description for the notebook.
* Enter this Notebook URL: (https://github.com/IBM/watson-nao-robot/blob/master/notebooks/Robo_Notebook.ipynb)
* Click the `Create Notebook` button.

![](doc/source/images/create_notebook_from_url.png)

## 8. Add the data 

#### Add the data to the notebook
Use `Find and Add Data` (look for the `10/01` icon)
and its `Files` tab. From there you can click
`browse` and add data files from your computer.

> Note: The data file in the `data` directory - `Data.csv` has been downloaded from (https://www.ibm.com/communities/analytics/watson-analytics-blog/retail-sales-marketing-profit-cost/). Change the file name from "WA_Retail-SalesMarketing_-ProfitCost.csv" to "Data.csv" before reading it in DSX. Please visit the site for the terms and conditions for usage of the data.

<p align="center">
  <img width="350" height="250" src="doc/source/images/add_file.png" title="Intents">
</p>

## 9. Update the notebook with service credentials

#### Add the Object Storage credentials to the notebook
Select the cell below `2.1 Add your service credentials for Object Storage` section in the notebook to update
the credentials for Object Store. 

Use `Find and Add Data` (look for the `10/01` icon) and its `Files` tab. You should see the file names uploaded earlier. Make sure your active cell is the empty one created earlier. Select `Insert to code` (below your file name). Click `Insert Crendentials` from drop down menu.

![](doc/source/images/objectstorage_credentials.png)

#### Update the websocket URL in the notebook
In the cell below `7. Expose integration point with a websocket client` , update the websocket url noted in [section 5](#5-note-the-websocket-url) in the `start_websocket_listener` function.

![](doc/source/images/update_websocket_url.png)

## 10. Run the notebook

When a notebook is executed, what is actually happening is that each code cell in
the notebook is executed, in order, from top to bottom.

Each code cell is selectable and is preceded by a tag in the left margin. The tag
format is `In [x]:`. Depending on the state of the notebook, the `x` can be:

* A blank, this indicates that the cell has never been executed.
* A number, this number represents the relative order this code step was executed.
* A `*`, this indicates that the cell is currently executing.

There are several ways to execute the code cells in your notebook:

* One cell at a time.
  * Select the cell, and then press the `Play` button in the toolbar.
* Batch mode, in sequential order.
  * From the `Cell` menu bar, there are several options available. For example, you
    can `Run All` cells in your notebook, or you can `Run All Below`, that will
    start executing from the first cell under the currently selected cell, and then
    continue executing all cells that follow.

## 11. Results sent to the Node Red Flow

