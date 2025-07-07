# Developing Cloud-Native Applications Using Microsoft Azure Cosmos DB - Connect & Configure to Azure Cosmos DB for NoSQL with the SDK

### Overall Estimated Duration: 120 Minutes

## Overview

This lab focuses on connecting to an Azure Cosmos DB SQL API account using the Azure SDK for .NET. Participants will prepare a development environment, create a Cosmos DB SQL API account, and integrate the Microsoft.Azure.Cosmos library into a .NET project. The lab includes importing the library, utilizing its features to interact with Cosmos DB, and testing the script to validate functionality. This lab provides a hands-on approach to building and testing .NET applications that leverage Azure Cosmos DB for managing non-relational data.

## Objective

Equip participants with the skills to connect to an Azure Cosmos DB SQL API account using the Azure SDK for .NET. By the end of this lab, you will be able to:

- **Connect to Azure Cosmos DB for NoSQL with the SDK:** The objective of this lab is to enable participants to connect to an Azure Cosmos DB SQL API account using the Azure SDK for .NET, and integrate the Microsoft.Azure.Cosmos library into a .NET project, and build and test scripts to interact with Azure Cosmos DB for managing non-relational data.

- **Configure the Azure Cosmos DB NoSQL API SDK for offline development:** The objective of this lab is to connect to an Azure Cosmos DB SQL API account using the Azure SDK for .NET and interact with non-relational data through the Azure Cosmos DB Emulator.

## Prerequisites

Participants should have:

- **Azure Platform Understanding:** Basic knowledge of Azure services, including Azure Cosmos DB.

- **Basic Programming Skills:** Familiarity with .NET development and C# programming.

## Architechture

This diagram illustrates the process of integrating and interacting with Azure Cosmos DB for NoSQL development using the Azure SDK and Emulator. The first workflow demonstrates how to connect to an Azure Cosmos DB NoSQL API account, set up the development environment, and test scripts using the Microsoft.Azure.Cosmos library. The second workflow highlights offline development by configuring the Azure Cosmos DB Emulator, connecting to it from the SDK, and creating and managing containers locally.

## Architecture Diagram

![](/instructions/architecturedia/archm3.png)

## Explanation of Components

- **Azure Cosmos DB:** A globally distributed, multi-model database service that supports MongoDB API for seamless integration. Cosmos DB provides scalability, high availability, and low latency. 

- **Visual Studio Code (VS Code):** A lightweight, versatile, and open-source code editor developed by Microsoft. It supports multiple programming languages, features extensions for enhanced functionality, and offers debugging, Git integration, and intelligent code completion.

## Getting started with the lab

Welcome to your Developing Cloud-Native Applications Using Microsoft Azure Cosmos DB! We've prepared a seamless environment designed to facilitate hands-on learning and exploration of Microsoft Azure Cosmos DB for building cloud-native applications. Let's begin by making the most of this experience:
 
## Accessing Your Lab Environment
 
Once you're ready to dive in, your virtual machine and guide will be right at your fingertips within your web browser.
 
![Access Your VM and Lab Guide](./instructions/media/25-06-25-g3-1.png)

### Virtual Machine & Lab Guide
 
In the integrated environment, the lab VM serves as the designated workspace, while the guide is accessible on the right side of the screen.

**Note**: Kindly ensure that you are following the instructions carefully to ensure the lab runs smoothly and provides an optimal user experience.
 
## Exploring Your Lab Resources
 
To get a better understanding of your lab resources and credentials, navigate to the **Environment** tab.

![Explore Lab Resources](./instructions/25-06-25-g2.1.png)

## Utilizing the Split Window Feature
 
For convenience, you can open the guide in a separate window by selecting the **Split Window** button from the Top right corner.
 
![Use the Split Window Feature](./instructions/25-06-25-g3.png)

## Managing Your Virtual Machine
 
Feel free to **start, stop, or restart** your virtual machine as needed from the **Resources** tab. Your experience is in your hands!
 
![Manage Your Virtual Machine](./instructions/25-06-25-g4%20.png)

## Lab Guide Zoom In/Zoom Out

To adjust the zoom level for the environment page, click the **A↕: 100%** icon located next to the timer in the lab environment.

![](./instructions/24-06-25-g5.png)

## **Lab Duration Extension**

1. To extend the duration of the lab, kindly click the **Hourglass** icon in the top right corner of the lab environment. 

   ![Use the Split Window Feature](./instructions/25-06-25-g1-hr.png)
   
   >**Note:** You will get the **Hourglass** icon when 10 minutes are remaining in the lab.

3. Click **OK** to extend your lab duration.
 
   ![Use the Split Window Feature](./instructions/media/gext2.png)

4. If you have not extended the duration prior to when the lab is about to end, a pop-up will appear, giving you the option to extend. Click **OK** to proceed.

## Let's Get Started with Azure Portal
 
1. In the **JumpVM**, click on the **Azure portal shortcut** of the Microsoft Edge browser, which is created on the desktop.

   ![](./instructions/media/azureportal.png)

1. On the **Sign in to Microsoft Azure** tab, you will see the login screen, in that enter the following email/username, and click on **Next**.
 
   - **Email/Username:** <inject key="AzureAdUserEmail"></inject>

     ![](instructions/media/intro1.png)

1. Now enter the following password and click on **Sign in**.
 
   - **Password:** <inject key="AzureAdUserPassword"></inject>

     ![](instructions/media/intro2.png)

1. If prompted to stay signed in, you can click **No"".

   ![](./instructions/media/Sign-in-no.png)
 
1. If a **Welcome to Microsoft Azure** pop-up window appears, simply click **cancel** to skip the tour.

## Support Contact

The CloudLabs support team is available 24/7, 365 days a year, via email and live chat to ensure seamless assistance at any time. We offer dedicated support channels tailored specifically for both learners and instructors, ensuring that all your needs are promptly and efficiently addressed.

Learner Support Contacts:

- Email Support: cloudlabs-support@spektrasystems.com

- Live Chat Support: https://cloudlabs.ai/labs-support
   
Now, click on Next from the lower right corner to move to the next page.

![](./instructions/media/num.png)

### Happy Learning!!
