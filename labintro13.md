# Developing Cloud-Native Applications Using Microsoft Azure Cosmos DB - Create a stored procedure with the Azure portal

### Overall Estimated Duration: 30 Minutes

## Overview

Stored procedures in Azure Cosmos DB provide a way to perform server-side logic and batch operations on your data. In this lab, you will learn how to create, optimize, and validate a stored procedure in your Azure Cosmos DB container. By leveraging stored procedures, you can execute operations efficiently and reduce latency for your database applications.

## Objective

Learn how to build an Azure Cognitive Search index that integrates seamlessly with Azure Cosmos DB for NoSQL and enriches data using Azure Cognitive Services Translator. By the end of this lab, you will be able to:

- **Create a stored procedure with the Azure portal:** The goal of this lab is to provide a comprehensive understanding of how to design and optimize indexing policies in Azure Cosmos DB for NoSQL. You will explore how to create, modify, and fine-tune indexing policies to improve query performance and efficiently handle large datasets. By the end of this lab, you will have the expertise to implement and evaluate different indexing strategies, measure their effects on database performance, and apply best practices for efficient data management.

## Prerequisites

Participants should have:

- **Azure Platform Understanding:** Basic knowledge of Azure services, including Azure Cosmos DB and Azure AI Search.

## Architechture

This diagram outlines the process of managing indexing policies in Azure Cosmos DB for NoSQL. It begins with setting up a Cosmos DB account and seeding it with sample data. Next, it demonstrates configuring the default indexing policy, followed by adjusting the policy to improve performance. Finally, it illustrates running a test .NET application to evaluate the performance of both the default and optimized indexing policies, ensuring effective data handling and query execution.

## Architechture Diagram

![image](/instructions/architecturedia/lab31.png)

## Explanation of Components

- **Azure Cosmos DB:** A globally distributed, multi-model database service that supports MongoDB API for seamless integration. Cosmos DB provides scalability, high availability, and low latency. 

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
