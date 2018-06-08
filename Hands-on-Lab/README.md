# Big data and visualization hands-on lab

AdventureWorks Travel (AWT) provides concierge services for business travelers. In an increasingly crowded market, they are always looking for ways to differentiate themselves, and provide added value to their corporate customers.

They are looking to pilot a web app that their internal customer service agents can use to provide additional information useful to the traveler during the flight booking process. They want to enable their agents to enter in the flight information and produce a prediction as to whether the departing flight will encounter a 15-minute or longer delay, considering the weather forecasted for the departure hour.

## Contents

* [Abstract](#abstract)
* [Solution architecture](#solution-architecture)
* [Requirements](#requirements)
* [Before the Hands-on Lab](#before-the-hands-on-lab)
* [Hands-on Lab](#hands-on-lab)
* [After the Hands-on Lab](#after-the-hands-on-lab)

## Abstract

In this workshop, you will deploy a web app using Machine Learning (ML) to predict travel delays given flight delay data and weather conditions. Plan a bulk data import operation, followed by preparation, such as cleaning and manipulating the data for testing, and training your Machine Learning model.

By attending this workshop, you will be better able to build a complete Azure Machine Learning (ML) model for predicting if an upcoming flight will experience delays. In addition, you will learn to:

* Integrate the Azure ML web service in a Web App for both one at a time and batch predictions

* Use Azure Data Factory (ADF) for data movement and operationalizing ML scoring

* Summarize data with HDInsight and Spark SQL

* Visualize batch predictions on a map using Power BI

This hands-on lab is designed to provide exposure to many of Microsoft's transformative line of business applications built using Microsoft big data and advanced analytics. The goal is to show an end-to-end solution, leveraging many of these technologies, but not necessarily doing work in every component possible. The lab architecture is below and includes:

* Azure Machine Learning (Azure ML)

* Azure Data Factory (ADF)

* Azure Storage

* HDInsight Spark

* Power BI Desktop

* Azure App Service

## Solution architecture

Below is a diagram of the solution architecture you will build in this lab. Please study this carefully so you understand the whole of the solution as you are working on the various components.

![The Solution Architecture diagram begins with Lab VM, then flows to Data Factory File Copy Pipeline, which flows to Storage for copied, raw file. This flows to Data Factory Batch Scoring pipeline, which includes Deployed ML Predictive Model (Batch). The pipeline flows to Storage for scored data, which flows to Spark for data processing. Power BI Report reads data from Spark, then sends the data on to Flight Booking Web App. Deployed ML Predictive Model (Request/Response) real-time scoring also sends data to the Flight Booking Web App, which then flows to the End User.](media/image2.png 'Solution Architecture diagram')

The solution begins with loading their historical data into blob storage using Azure Data Factory (ADF). By setting up a pipeline containing a copy activity configured to copy time partitioned source data, they could pull all their historical information, as well as ingest any future data, into Azure blob storage through a scheduled, and continuously running pipeline. Because their historical data is stored on-premises, AWT would need to install and configure an Azure Data Factory Integration Runtime (formerly known as a Data Management Gateway). Azure Machine Learning (Azure ML) would be used to develop a two-class classification machine learning model, which would then be operationalized as a Predictive Web Service using ML Studio. After operationalizing the ML model, a second ADF pipeline, using a Linked Service pointing to Azure ML's Batch Execution API and an AzureMLBatchExecution activity, would be used to apply the operational model to data as it is moved to the proper location in Azure storage. The scored data in Azure storage can be explored and prepared using Spark SQL on HDInsight, and the results visualized using a map visualization in Power BI.

## Requirements

* Microsoft Azure subscription must be pay-as-you-go or MSDN
  * Trial subscriptions will not work

## Before the Hands-on Lab

Before attending the hands-on lab workshop, you should set up your environment for use in the rest of the hands-on lab.

You should follow all the steps provided in the [Before the Hands-on Lab](./Setup.md) section to prepare your environment before attending the hands-on lab. Failure to complete the Before the Hands-on Lab setup may result in an inability to complete the lab with in the time allowed.

## Hands-on Lab

Select the guide you are using to complete the Hands-on lab below.

* [Step-by-step guide](./Step-by-step.md)
  * Provides detailed, step-by-step instructions for completing the lab.
* [Unguided](./Unguided.md)
  * This guide provides minimal instruction, and assumes a high-level of knowledge about the technologies used in this lab. This should typically only be used if you are doing this as part of a group or hackathon.
* [Hackaton](./Hack.md)

## After the Hands-on Lab

After completing the hands-on lab, you should delete any Azure resources that were created in support of the lab.

You should follow all the steps in the [After the Hands-on Lab](./clean-up.md) section after completing the Hands-on lab.
