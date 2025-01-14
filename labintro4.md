# Developing Cloud-Native Applications Using Microsoft Azure Cosmos DB - Create and update documents with the Azure Cosmos DB for NoSQL SDK

### Overall Estimated Duration: 60 Minutes

## Overview

This lab focuses on leveraging the Azure Cosmos DB SDK to perform common CRUD (Create, Read, Update, Delete) operations on items stored within an Azure Cosmos DB for NoSQL container. By completing the lab, you will gain hands-on experience with configuring and interacting with an Azure Cosmos DB instance, and performing essential data operations programmatically.

## Objective

Equip participants with the skills to connect to an Azure Cosmos DB NoSQL API account using the Azure SDK for .NET. By the end of this lab, you will be able to:

- **Create and update documents with the Azure Cosmos DB NoSQL API SDK:** The objective of this lab is to enable participants to connect to an Azure Cosmos DB SQL API account using the Azure SDK for .NET, integrate the Microsoft.Azure.Cosmos library into a .NET project, and build and test scripts to efficiently perform CRUD operations for managing non-relational data in Azure Cosmos DB.

## Prerequisites

Participants should have:

- **Azure Platform Understanding:** Basic knowledge of Azure services, including Azure Cosmos DB.

- **Basic Programming Skills:** Familiarity with .NET development and C# programming.

## Architechture

This diagram illustrates the process of performing CRUD operations on Azure Cosmos DB for NoSQL using the Azure SDK. The first workflow outlines creating an Azure Cosmos DB NoSQL API account, configuring the SDK, and connecting to the database. The second workflow details the steps for performing create, read, update, and delete operations on items within a Cosmos DB container, leveraging the SDK for seamless interaction with non-relational data.

## Architechture Diagram

![image](/instructions//architecturedia/lab6.png)

## Explanation of Components

- **Azure Cosmos DB:** A globally distributed, multi-model database service that supports MongoDB API for seamless integration. Cosmos DB provides scalability, high availability, and low latency. 

- **Visual Studio Code (VS Code):** A lightweight, versatile, and open-source code editor developed by Microsoft. It supports multiple programming languages, features extensions for enhanced functionality, and offers debugging, Git integration, and intelligent code completion.

## Getting started with the lab

Welcome to your Developing Cloud-Native Applications Using Microsoft Azure Cosmos DB! We've prepared a seamless environment designed to facilitate hands-on learning and exploration of Microsoft Azure Cosmos DB for building cloud-native applications. Let's begin by making the most of this experience:
 
## Accessing Your Lab Environment
 
Once you're ready to dive in, your virtual machine and lab guide will be right at your fingertips within your web browser.
 
![Access Your VM and Lab Guide](./instructions/media/labguide.png)

### Virtual Machine & Lab Guide
 
Your virtual machine is your workhorse throughout the workshop. The lab guide is your roadmap to success.
 
## Exploring Your Lab Resources
 
To get a better understanding of your lab resources and credentials, navigate to the **Environment Details** tab.
 
![Explore Lab Resources](./instructions/media/env.png)

## Utilizing the Split Window Feature
 
For convenience, you can open the lab guide in a separate window by selecting the **Split Window** button from the Top right corner.
 
![Use the Split Window Feature](./instructions/media/spl.png)
 
## **Lab Duration Extension**

1. To extend the duration of the lab, kindly click the **Hourglass** icon in the top right corner of the lab environment. 

   ![Use the Split Window Feature](./instructions/media/gext.png)   

   >**Note:** You will get the **Hourglass** icon when 10 minutes are remaining in the lab.

3. Click **OK** to extend your lab duration.
 
   ![Use the Split Window Feature](./instructions/media/gext2.png)

4. If you have not extended the duration prior to when the lab is about to end, a pop-up will appear, giving you the option to extend. Click **OK** to proceed.

## Managing Your Virtual Machine
 
Feel free to start, stop, or restart your virtual machine as needed from the **Resources** tab. Your experience is in your hands!
 
![Manage Your Virtual Machine](./instructions/media/res.png)

## Let's Get Started with Azure Portal
 
1. On your virtual machine, click on the Azure Portal icon as shown below:
   ![](media/azureportal.png)

1. Log in to Azure Portal.

1. You'll see the **Sign into Microsoft Azure** tab. Here, enter your credentials:
 
   - **Email/Username:** <inject key="AzureAdUserEmail"></inject>

     ![](instructions/media/intro1.png)

1. Next, provide your password:
 
   - **Password:** <inject key="AzureAdUserPassword"></inject>

     ![](instructions/media/intro2.png)

1. If prompted to stay signed in, you can click "No."
 
1. If a **Welcome to Microsoft Azure** pop-up window appears, simply click "cancel" to skip the tour.

## Support Contact

The CloudLabs support team is available 24/7, 365 days a year, via email and live chat to ensure seamless assistance at any time. We offer dedicated support channels tailored specifically for both learners and instructors, ensuring that all your needs are promptly and efficiently addressed.

Learner Support Contacts:

- Email Support: cloudlabs-support@spektrasystems.com

- Live Chat Support: https://cloudlabs.ai/labs-support
   
Now, click on Next from the lower right corner to move to the next page.

![](./instructions/media/num.png)

### Happy Learning!!
