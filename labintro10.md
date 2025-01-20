# Developing Cloud-Native Applications Using Microsoft Azure Cosmos DB - Search data using Azure Cognitive Search and Azure Cosmos DB for NoSQL

### Overall Estimated Duration: 30 Minutes

## Overview

In this lab, you will learn how to build an Azure Cognitive Search index that automatically integrates with data stored in an Azure Cosmos DB SQL API container. Additionally, you will use the Azure Cognitive Services Translator to enrich the indexed data. This lab provides hands-on experience in setting up and validating a seamless data indexing and enrichment pipeline.

## Objective

Learn how to build an Azure Cognitive Search index that integrates with Azure Cosmos DB for NoSQL and enriches data using Azure Cognitive Services Translator. By the end of this lab, you will be able to:

- **Search data using Azure AI Search and Azure Cosmos DB for NoSQL:** The objective of this lab is to provide a comprehensive understanding of how to build an Azure Cognitive Search index integrated with Azure Cosmos DB for NoSQL. You will learn to configure an indexer to automatically index and enrich data using Azure Cognitive Services Translator. By the end of the lab, you will have the skills to integrate Azure Cognitive Search with Cosmos DB, enrich data for advanced search scenarios, and validate the functionality with search queries..

## Prerequisites

Participants should have:

- **Azure Platform Understanding:** Basic knowledge of Azure services, including Azure Cosmos DB and Azure AI Search.

- **Basic Programming Skills:** Familiarity with .NET development and C# programming.

## Architechture

This diagram illustrates the process of building an Azure Cognitive Search index integrated with Azure Cosmos DB for NoSQL. It begins with creating a Cosmos DB account and seeding it with sample data. Next, it shows how to set up an Azure Cognitive Search resource, configure an indexer to fetch and index data automatically, and enrich the data using Azure Cognitive Services Translator, enabling advanced search capabilities and efficient data management.

## Architechture Diagram

![image](/instructions/architecturedia/lab15.png)

## Explanation of Components

- **Azure Cosmos DB:** A globally distributed, multi-model database service that supports MongoDB API for seamless integration. Cosmos DB provides scalability, high availability, and low latency. 

- **Visual Studio Code (VS Code):** A lightweight, versatile, and open-source code editor developed by Microsoft. It supports multiple programming languages, features extensions for enhanced functionality, and offers debugging, Git integration, and intelligent code completion.

- **Azure AI Search:** A fully managed search service that allows you to build intelligent search experiences. It combines powerful indexing and AI capabilities to analyze and enrich data, providing fast and relevant search results from both structured and unstructured data.

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
