![](images/HeaderPic.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Big data and visualization
</div>

<div class="MCWHeader2">
Hands-on lab unguided
</div>

<div class="MCWHeader3">
April 2018
</div>



Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.
© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [Big data and visualization hands-on lab unguided](#big-data-and-visualization-hands-on-lab-unguided)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Overview](#overview)
    - [Solution architecture](#solution-architecture)
    - [Requirements](#requirements)
    - [Exercise 1: Build a Machine Learning Model](#exercise-1-build-a-machine-learning-model)
        - [Task 1: Navigate to Machine Learning Studio](#task-1-navigate-to-machine-learning-studio)
            - [Tasks to complete:](#tasks-to-complete)
            - [Exit criteria:](#exit-criteria)
        - [Task 2: Upload the sample datasets](#task-2-upload-the-sample-datasets)
            - [Tasks to complete:](#tasks-to-complete-1)
            - [Exit criteria:](#exit-criteria-1)
        - [Task 3: Start a new experiment](#task-3-start-a-new-experiment)
            - [Tasks to complete:](#tasks-to-complete-2)
            - [Exit criteria:](#exit-criteria-2)
        - [Task 4: Prepare flight delay data](#task-4-prepare-flight-delay-data)
            - [Tasks to complete:](#tasks-to-complete-3)
            - [Exit criteria:](#exit-criteria-3)
        - [Task 5: Prepare the weather data](#task-5-prepare-the-weather-data)
            - [Tasks to complete:](#tasks-to-complete-4)
            - [Exit criteria:](#exit-criteria-4)
        - [Task 6: Join the flight and weather datasets](#task-6-join-the-flight-and-weather-datasets)
            - [Tasks to complete:](#tasks-to-complete-5)
            - [Exit criteria:](#exit-criteria-5)
        - [Task 7: Train the model](#task-7-train-the-model)
            - [Tasks to complete:](#tasks-to-complete-6)
            - [Exit criteria:](#exit-criteria-6)
        - [Task 8: Operationalize the experiment](#task-8-operationalize-the-experiment)
            - [Tasks to complete:](#tasks-to-complete-7)
            - [Exit criteria:](#exit-criteria-7)
    - [Exercise 2: Setup Azure Data Factory](#exercise-2-setup-azure-data-factory)
        - [Task 1: Connect to the Lab VM](#task-1-connect-to-the-lab-vm)
            - [Tasks to complete:](#tasks-to-complete-8)
            - [Exit criteria:](#exit-criteria-8)
        - [Task 2: Download and stage data to be processed](#task-2-download-and-stage-data-to-be-processed)
            - [Tasks to complete:](#tasks-to-complete-9)
            - [Exit criteria:](#exit-criteria-9)
        - [Task 3: Install and configure Azure Data Factory Integration Runtime on the lab VM](#task-3-install-and-configure-azure-data-factory-integration-runtime-on-the-lab-vm)
            - [Tasks to complete:](#tasks-to-complete-10)
            - [Exit criteria:](#exit-criteria-10)
        - [Task 4: Create an Azure data factory](#task-4-create-an-azure-data-factory)
            - [Tasks to complete:](#tasks-to-complete-11)
            - [Exit criteria:](#exit-criteria-11)
    - [Exercise 3: Develop a data factory pipeline for data movement](#exercise-3-develop-a-data-factory-pipeline-for-data-movement)
        - [Task 1: Create copy pipeline using the Copy Data Wizard](#task-1-create-copy-pipeline-using-the-copy-data-wizard)
            - [Tasks to complete:](#tasks-to-complete-12)
            - [Exit criteria:](#exit-criteria-12)
    - [Exercise 4: Operationalize ML scoring with Azure ML and Data Factory](#exercise-4-operationalize-ml-scoring-with-azure-ml-and-data-factory)
        - [Task 1: Create Azure ML Linked Service](#task-1-create-azure-ml-linked-service)
            - [Tasks to complete:](#tasks-to-complete-13)
            - [Exit criteria:](#exit-criteria-13)
        - [Task 2: Create Azure ML input dataset](#task-2-create-azure-ml-input-dataset)
            - [Tasks to complete:](#tasks-to-complete-14)
            - [Exit criteria:](#exit-criteria-14)
        - [Task 3: Create Azure ML scored dataset](#task-3-create-azure-ml-scored-dataset)
            - [Tasks to complete:](#tasks-to-complete-15)
            - [Exit criteria:](#exit-criteria-15)
        - [Task 4: Create Azure ML predictive pipeline](#task-4-create-azure-ml-predictive-pipeline)
            - [Tasks to complete:](#tasks-to-complete-16)
            - [Exit criteria:](#exit-criteria-16)
        - [Task 5: Monitor pipeline activities](#task-5-monitor-pipeline-activities)
            - [Tasks to complete:](#tasks-to-complete-17)
            - [Exit criteria:](#exit-criteria-17)
    - [Exercise 5: Summarize data using HDInsight Spark](#exercise-5-summarize-data-using-hdinsight-spark)
        - [Task 1: Install pandas on the HDInsight cluster](#task-1-install-pandas-on-the-hdinsight-cluster)
            - [Tasks to complete:](#tasks-to-complete-18)
            - [Exit criteria:](#exit-criteria-18)
        - [Task 2: Summarize delays by airport](#task-2-summarize-delays-by-airport)
            - [Tasks to complete:](#tasks-to-complete-19)
            - [Exit criteria:](#exit-criteria-19)
    - [Exercise 6: Visualizing in Power BI Desktop](#exercise-6-visualizing-in-power-bi-desktop)
        - [Task 1: Connect to the Lab VM](#task-1-connect-to-the-lab-vm-1)
            - [Tasks to complete:](#tasks-to-complete-20)
            - [Exit criteria:](#exit-criteria-20)
        - [Task 2: Connect to HDInsight Spark using Power BI Desktop](#task-2-connect-to-hdinsight-spark-using-power-bi-desktop)
            - [Tasks to complete:](#tasks-to-complete-21)
            - [Exit criteria:](#exit-criteria-21)
        - [Task 3: Create Power BI report](#task-3-create-power-bi-report)
            - [Tasks to complete:](#tasks-to-complete-22)
            - [Exit criteria:](#exit-criteria-22)
    - [Exercise 7: Deploy intelligent web app](#exercise-7-deploy-intelligent-web-app)
        - [Task 1: Deploy web app from GitHub](#task-1-deploy-web-app-from-github)
            - [Tasks to complete:](#tasks-to-complete-23)
            - [Exit criteria:](#exit-criteria-23)
    - [After the hands-on lab](#after-the-hands-on-lab)
        - [Task 1: Delete resource group](#task-1-delete-resource-group)

<!-- /TOC -->


# Big data and visualization hands-on lab unguided 

## Abstract and learning objectives 

In this workshop, you will deploy a web app using Machine Learning (ML) to predict travel delays given flight delay data and weather conditions. Plan a bulk data import operation, followed by preparation, such as cleaning and manipulating the data for testing, and training your Machine Learning model.

By attending this workshop, you will be better able to build a complete Azure Machine Learning (ML) model for predicting if an upcoming flight will experience delays. In addition, you will learn to:

-   Integrate the Azure ML web service in a Web App for both one at a time and batch predictions

-   Use Azure Data Factory (ADF) for data movement and operationalizing ML scoring

-   Summarize data with HDInsight and Spark SQL

-   Visualize batch predictions on a map using Power BI

This hands-on lab is designed to provide exposure to many of Microsoft's transformative line of business applications built using Microsoft big data and advanced analytics. The goal is to show an end-to-end solution, leveraging many of these technologies, but not necessarily doing work in every component possible. The lab architecture is below and includes:

-   Azure Machine Learning (Azure ML)

-   Azure Data Factory (ADF)

-   Azure Storage

-   HDInsight Spark

-   Power BI Desktop

-   Azure App Service

## Overview

AdventureWorks Travel (AWT) provides concierge services for business travelers. In an increasingly crowded market, they are always looking for ways to differentiate themselves, and provide added value to their corporate customers.

They are looking to pilot a web app that their internal customer service agents can use to provide additional information useful to the traveler during the flight booking process. They want to enable their agents to enter in the flight information and produce a prediction as to whether the departing flight will encounter a 15-minute or longer delay, considering the weather forecasted for the departure hour.

In this hands-on lab, attendees will build an end-to-end solution to predict flight delays, accounting for the weather forecast.

## Solution architecture

Below is a diagram of the solution architecture you will build in this lab. Please study this carefully so you understand the whole of the solution as you are working on the various components.

![The Solution Architecture diagram begins with Lab VM, then flows to Data Factory File Copy Pipeline, which flows to Storage for copied, raw file. This flows to Data Factory Batch Scoring pipeline, which includes Deployed ML Predictive Model (Batch). The pipeline flows to Storage for scored data, which flows to Spark for data processing. Power BI Report reads data from Spark, then sends the data on to Flight Booking Web App. Deployed ML Predictive Model (Request/Response) real-time scoring also sends data to the Flight Booking Web App, which then flows to the End User.](images/Hands-onlabunguided-Bigdataandvisualizationimages/media/image2.png "Solution Architecture diagram")

The solution begins with loading their historical data into blob storage using Azure Data Factory (ADF). By setting up a pipeline containing a copy activity configured to copy time partitioned source data, they could pull all their historical information, as well as ingest any future data, into Azure blob storage through a scheduled, and continuously running pipeline. Because their historical data is stored on-premises, AWT would need to install and configure an Azure Data Factory Integration Runtime (formerly known as a Data Management Gateway). Azure Machine Learning (Azure ML) would be used to develop a two-class classification machine learning model, which would then be operationalized as a Predictive Web Service using ML Studio. After operationalizing the ML model, a second ADF pipeline, using a Linked Service pointing to Azure ML's Batch Execution API and an AzureMLBatchExecution activity, would be used to apply the operational model to data as it is moved to the proper location in Azure storage. The scored data in Azure storage can be explored and prepared using Spark SQL on HDInsight, and the results visualized using a map visualization in Power BI.

## Requirements

1.  Microsoft Azure subscription must be pay-as-you-go or MSDN

    a.  Trial subscriptions will not work

## Exercise 1: Build a Machine Learning Model

Duration: 60 minutes

In this exercise, attendees will implement a classification experiment. They will load the training data from their local machine into a dataset. Then, they will explore the data to identify the primary components they should use for prediction, and use two different algorithms for predicting the classification. They will evaluate the performance of both and algorithms choose the algorithm that performs best. The model selected will be exposed as a web service that is integrated with the sample web app.

### Task 1: Navigate to Machine Learning Studio

#### Tasks to complete:

-   Launch ML Studio.

#### Exit criteria:

-   You have an open ML Studio session in your browser.

### Task 2: Upload the sample datasets

#### Tasks to complete:

-   Download three CSV sample datasets from <http://bit.ly/2wGAqrl>.

-   Extract the ZIP, and verify you have the following files:

    -   FlightDelaysWithAirportCodes.csv

    -   FlightWeatherWithAirportCodes.csv

    -   AirportCodeLocationClean.csv

-   Upload the sample CSVs as Datasets in ML Studio.

#### Exit criteria:

-   The three sample CSV files are uploaded and available as datasets in ML Studio. 

    ![In the left pane of the Microsoft Azure Machine Learning page, Datasets is selected. In the right pane, under My Datasets, three .csv files display: AirportCodeLocationLookupClean, FlightDelaysWithAirportCodes, and FlightWeatherWithAirportCode.](images/Hands-onlabunguided-Bigdataandvisualizationimages/media/image26.png "Microsoft Azure Machine Learning  page")

### Task 3: Start a new experiment

#### Tasks to complete:

-   Create a new blank Experiment in ML Studio. You should follow the detailed step-by-step instructions for this Task, available in the step-by-step guide.

-   Provide the experiment a name, such as AdventureWorks Travel.

#### Exit criteria:

-   You have a new named experiment.

### Task 4: Prepare flight delay data

#### Tasks to complete:

-   Add the FlightDelaysWithAirportCodes dataset to your experiment.

-   Prepare the data using an Execute R Script module.

    -   Remove rows with missing values

    -   Generate a new column, named "CRSDepHour," which contains the rounded down value representing just the hour from CRSDepTime

    -   Pare down columns to only those needed for our model: \"OriginAirportCode\",\"OriginLatitude\", \"OriginLongitude\", \"Month\", \"DayofMonth\", \"CRSDepHour\", \"DayOfWeek\", \"Carrier\", \"DestAirportCode\", \"DestLatitude\", \"DestLongitude\", \"DepDel15\"

#### Exit criteria:

-   You have an Experiment with cleaned up data from the FlightDelaysWithAirportCodes dataset.

### Task 5: Prepare the weather data

#### Tasks to complete:

-   Update the ML Experiment within ML Studio to prepare the FlightWeatherWithAirportCodes dataset.

-   Prepare the data using an Execute Python Script module.

    -   WindSpeed: Replace missing values with 0.0, and "M" values with 0.005

    -   HourlyPrecip: Replace missing values with 0.0, and "T" values with 0.005

    -   SeaLevelPressure: Replace "M" values with 29.92 (the average pressure)

    -   Convert WindSpeed, HourlyPrecip, and SeaLevelPressure to numeric columns

    -   Round "Time" column down to the nearest hour, and add value to a new column named "Hour"

    -   Eliminate unneeded columns from the dataset: \'AirportCode\', \'Month\', \'Day\', \'Hour\', \'WindSpeed\', \'SeaLevelPressure\', \'HourlyPrecip\'

#### Exit criteria:

-   You have an experiment with munged data from the FlightWeatherWithAirportCodes dataset.

### Task 6: Join the flight and weather datasets

#### Tasks to complete:

-   Join the two datasets -the prepared data from the FlightDelaysWithAirportCodes and FlightWeatherWithAirportCodes datasets. Join on these columns:

    -   Left columns: OriginAirportCode, Month, DayofMonth, and CRSDepHour

    -   Right columns: AirportCode, Month, Day, and Hour

-   Convert the following columns to categorical:

    -   DayOfWeek, Carrier, DestAirportCode, and OriginAirportCode

-   Omit the following columns:

    -   OriginLatitude, OriginLongitude, DestLatitude, and DestLongitude from the joined data.

#### Exit criteria:

-   You have an experiment that has a single dataset, which provides information about flights and weather at the time of departure.

-   Your data should look like:

    ![The Results dataset displays data statistics and visualizations for airports.](images/Hands-onlabunguided-Bigdataandvisualizationimages/media/image27.png "Results dataset")

-   The model should look like:

    ![The Data model diagram begins at the top with FlightDelaysWithAirportCodes on the left, and FlightWeatherWithAirportCodes on the right. FlightDelaysWithAirportCodes points down to Execute R Script, which points to Join Data. FlightWeatherWithAirportCodes on the right points down to Execute Python Script, which also points to Join Data. Join Data then points to Edit Metadata, which points to Select Columns in Dataset.](images/Hands-onlabunguided-Bigdataandvisualizationimages/media/image28.png "Predictive Experiment data model diagram")

### Task 7: Train the model

#### Tasks to complete:

-   Complete the experiment by training a model using a Two-Class Logistic Regression.

    -   Split the data so 70% is used for training and 30% is used for testing.

    -   Score the model

    -   Evaluate the model performance

-   For this task, you should follow the detailed steps in the step-by-step guide for this lab.

#### Exit criteria:

-   You should be able to evaluate your model's performance, and verify that its predictions are performing better than random.

-   Use the Score Model module and select **Visualize** to see the results of its predictions. **You should have a total of 13 columns**.

    ![Prediction results for AdventureWorks Travel display.](images/Hands-onlabunguided-Bigdataandvisualizationimages/media/image29.png "Prediction results")

-   Examining the visualization for the Evaluate model, you should see performance similar to the following:\
    
    ![Evaluation results for Adventureworks Travel display in line graph format, with True Positive Rate over False Positive Rate. The line curves upward from left to right.](images/Hands-onlabunguided-Bigdataandvisualizationimages/media/image30.png "Evaluation results ")

### Task 8: Operationalize the experiment

#### Tasks to complete:

-   Operationalize the model by packaging it up as a Predictive Web Service.

-   Clean up the web service inputs and data flow:

    -   Connect the Web service input to the Edit Metadata below the Join Data module.

    -   In between the Join Data and the Metadata Editor modules, drop a Select Columns in Dataset module. Connect the Join Data module's output to the Select Columns module's input, and the Select Columns output to the Edit Metadata module's input.

        -   Exclude these columns: **DepDel15, OriginLatitude, OriginLongitude, DestLatitude,** and **DestLongitude**.

    -   Select the Select Columns in Dataset module that comes after the Metadata Editor module, and delete it.

    -   Connect the output of the Edit Metadata module directly to the right input of the Score Model module.

    -   Add the latitude and longitude columns from the data set back with a lookup:

        -   Drag the AirportCodeLocationLookupClean dataset on to the design surface, positioning it below and to the right of the Score Model module.

        -   Add a Join Data module, and position it below and to the left of the AirportCodeLocationLookupClean module. In the **Properties** panel for the Join Data module, for the Join key columns for L set the selected columns to **OriginAirportCode**. For the Join key columns for R, set the Selected columns to **AIRPORT**. Uncheck Keep right key columns in joined table.

    -   Connect the output of the Score Model module to the leftmost input of the Join Data module and the output of the dataset to the rightmost input of the Join Data module.

    -   Add a Select Columns in Dataset module beneath the Join Data module. In the Property panel, begin with All Columns, and set the Selected columns to Exclude the columns: AIRPORT\_ID and DISPLAY\_AIRPORT\_NAME.

    -   Connect the Join Data output to the input of the Select Columns in Dataset module.

    -   Add an Edit Metadata module. In the **Properties** panel for the Metadata Editor, use the column selector to set the Selected columns to LATITUDE and LONGITUDE. In the New column names enter: **OriginLatitude**, **OriginLongitude.**

    -   Connect the output of the Select Columns in Dataset module to the input of the Edit Metadata module.

    -   Connect the output of the Edit Metadata to the input of the web service output module.

-   Run the experiment

-   Select **Deploy Web Service**, **Deploy Web Service \[NEW\] Preview**

-   Deploy the web service

-   Navigate to the Consume tab of the deployed web service to acquire the Primary Key and Batch Requests URI .

#### Exit criteria:

-   Your predictive experiment should appear as follows:

    ![The Predictive Experiment diagram picks back up at Select Columns in Dataset. With Web service input, they both flow into Edit Metadata. With Big Data Hands-on Lab, they both flow into Score Model. With AirportCodeLocationLookup, they both flow into Join Data. Join Data flows down to Select Columns in Dataset, which flows down into Edit Metadata, and ends at Web Service output.](images/Hands-onlabunguided-Bigdataandvisualizationimages/media/image31.png "Predictive Experiment diagram")

-   You should be able to view the Web Service dashboard for your deployed Predictive Web Service, similar to the following:

    ![The Web Service dashboard for Big Data hands-on Lab \[Predictive Exp.\] displays.](images/Hands-onlabunguided-Bigdataandvisualizationimages/media/image32.png "Web Service dashboard")

## Exercise 2: Setup Azure Data Factory

Duration: 20 minutes

In this exercise, attendees will create a baseline environment for Azure Data Factory development for further operationalization of data movement and processing. You will create a Data Factory service, and then install the Data Management Gateway which is the agent that facilitates data movement from on-premises to Microsoft Azure.

### Task 1: Connect to the Lab VM

#### Tasks to complete:

-   Initiate an RDP connection to the Lab VM you created in the Before the Lab section.

#### Exit criteria:

-   You are logged into your Lab VM.

### Task 2: Download and stage data to be processed

#### Tasks to complete:

-   Download and extract the ZIP containing sample data to a folder named C:\\Data on your Lab VM. The data can be downloaded from <http://bit.ly/2zi4Sqa>.

#### Exit criteria:

-   You have a folder containing sample data files, partitioned by year and month on the C:\\ drive of your Lab VM.

### Task 3: Install and configure Azure Data Factory Integration Runtime on the lab VM

#### Tasks to complete:

-   Download and install the latest version of Azure Data Factory Integration Runtime from <https://www.microsoft.com/en-us/download/details.aspx?id=39717>

#### Exit criteria:

-   The Azure Data Factory Integration Runtime is installed and running on your system. Keep in open for now. We will come back to this screen once we have provisioned the Data Factory in Azure, and obtain the gateway key so we can connect Data Factory to this "on-premises" server.

    ![The Microsoft Integration Runtime Configuration Manager page displays.](images/Hands-onlabunguided-Bigdataandvisualizationimages/media/image33.png "Microsoft Integration Runtime Configuration Manager page")

### Task 4: Create an Azure data factory

#### Tasks to complete:

-   Provision a new Azure Data Factory (ADF) in your Azure subscription.

-   Create a new Integration Runtime (gateway), and connect it to the Azure Data Factory Integration Runtime running on your Lab VM.

#### Exit criteria:

-   You can navigate to the overview blade for ADF.

-   You have authored an Integration Runtime in ADF, and successfully connected it to the ADF Integration Runtime on your Lab VM. You should see a screen like the following:

    ![In the Microsoft Integration Runtime Configuration Manager, on the Home tab is a message that the Node is connected to the cloud service.](images/Hands-onlabunguided-Bigdataandvisualizationimages/media/image34.png "Microsoft Integration Runtime Configuration Manager ")


## Exercise 3: Develop a data factory pipeline for data movement

Duration: 20 minutes

In this exercise, you will create an Azure Data Factory pipeline to copy data (.CSV file) from an on-premises server (Lab VM) to Azure Blob Storage. The goal of the exercise is to demonstrate data movement from an on-premises location to Azure Storage (via the Integration Runtime). You will see how assets are created, deployed, executed, and monitored.

### Task 1: Create copy pipeline using the Copy Data Wizard

#### Tasks to complete:

-   Use the Copy Data (Preview) tool in ADF to generate a Copy Pipeline, moving data from your "on-premises" Lab VM, to Azure Storage account ending in "sparkstorage" that was provisioned in the lab setup.

    -   The pipeline should run regularly, once per month

    -   Start date is 03/01/2017 12:00 am

    -   For the source:

        -   Use a File System source

        -   Choose the path C:\\Data\\FlightsAndWeather

        -   Ensure files copied recursively

    -   For the destination:

        -   Use Azure Blob Storage

        -   Make sure you select the storage account with the **sparkstorage** suffix.

        -   The folder path should be something like: sparkcontainer/FlightsAndWeather/{yyyy}/{MM}/

        -   Add a header to the file

        -   Skip all incompatible rows

        -   Set the Copy Settings to have concurrency of 10 and execution priority order of OldestFirst.

    -   Deploy the pipeline.

#### Exit criteria:

-   The sample data copied to the C:\\ drive of your Lab VM has been successfully moved to Azure storage using an ADF copy activity and pipeline.

## Exercise 4: Operationalize ML scoring with Azure ML and Data Factory

Duration: 20 minutes

In this exercise, you will extend the Data Factory to operationalize the scoring of data using the previously created Azure Machine Learning (ML) model.

### Task 1: Create Azure ML Linked Service

#### Tasks to complete:

-   Create a new Azure ML Linked Service in ADF, and link it to your ML Predictive Web Service with the Batch Request URL and API key.

#### Exit criteria:

-   You have a Linked Service connected to your ML web service.

### Task 2: Create Azure ML input dataset

#### Tasks to complete:

-   Author a new ADF dataset for providing blob input to an ML Predictive pipeline.

#### Exit criteria:

-   You have a dataset connected to the storage location of the sample data uploaded by the Copy data pipeline created previously.

### Task 3: Create Azure ML scored dataset

#### Tasks to complete:

-   Author another ADF dataset, also connected to Azure Storage for outputting CSV files containing our sample data, along with Scored fields from our ML model.

-   The dataset should write all files to the same folder, and append the year and date to the file name.

#### Exit criteria:

-   Your ADF contains an output dataset pointing to a folder location in storage where all scored files can be written. The files should all be written to the same directory.

### Task 4: Create Azure ML predictive pipeline

#### Tasks to complete:

-   Create a new Pipeline in ADF, containing an AzureMLBatchExecution activity with the ML Input Dataset and ML Scored Dataset as input and output parameters.

#### Exit criteria:

-   Upon completion, you should be able to launch the Monitor & Manage window from ADF and observe your activities in a Ready state, and your scored data should reside in the target folder in your Azure storage account.

### Task 5: Monitor pipeline activities

#### Tasks to complete:

-   Launch the Monitor & Manage window from ADF, and observe your pipelines.

-   Ensure the pipelines are running, and the activities is in an In progress or Ready state.

#### Exit criteria:

-   You should see that your pipelines are In progress or completed (Ready).

## Exercise 5: Summarize data using HDInsight Spark

Duration: 20 minutes

In this exercise, you will prepare a summary of flight delay data in HDFS using Spark SQL.

### Task 1: Install pandas on the HDInsight cluster

#### Tasks to complete:

-   Create an SSH connection to the HDInsight cluster.

    -   Use password: Abc!1234567890

-   Install the latest version of pandas on the cluster

#### Exit criteria:

-   The version of pandas is updated to a version which supports the 'api' module.

### Task 2: Summarize delays by airport

#### Tasks to complete:

-   Navigate to your HDInsight Spark cluster in the Azure portal.

-   Open a new Jupyter Notebook, using a Spark kernel.

-   Generate Hive tables from your Scored Flight and Weather data, which can be queried using Spark SQL.

-   Load the data from the path \"/ScoredFlightsAndWeather/\*.csv\" and define a DataFrame with a schema and save it as a table called FlightDelays. The schema should look like the following:

    ![Screenshot of the FlightDelays schema.](images/Hands-onlabunguided-Bigdataandvisualizationimages/media/image35.png "FlightDelays schema")

-   Save a Hive table named FlightDelaysSummary that represents the following query:
    ```
    %%sql
    SELECT  OriginAirportCode, OriginLatLong, Month, Day, Hour, Sum(DelayPredicted) NumDelays, Avg(DelayProbability) AvgDelayProbability 
    FROM FlightDelays 
    WHERE Month = 4
    GROUP BY OriginAirportCode, OriginLatLong, Month, Day, Hour
    Having Sum(DelayPredicted) > 1
    ```

#### Exit criteria:

-   You can run the following query and see similar results:\
    ![The Query Results are in a table that display information for four airports. Columns are: OriginAirportCode, OriginLatLong, Month, Day, Hour, NumDelays, and AvgDelayProbability.](images/Hands-onlabunguided-Bigdataandvisualizationimages/media/image36.png "Query Results")

-   You have created flight delays summary Hive table, which can be queried from Power BI Desktop.

## Exercise 6: Visualizing in Power BI Desktop

### Task 1: Connect to the Lab VM

#### Tasks to complete:

-   Create an RDP connection to your Lab VM.

#### Exit criteria:

-   You are logged into your Lab VM.

### Task 2: Connect to HDInsight Spark using Power BI Desktop

#### Tasks to complete:

-   Launch Power BI Desktop.

-   Connect to your HDInsight Spark instance, and query the Hive tables you created in the previous exercise.

#### Exit criteria:

-   You have successfully connected to your HDInsight Spark cluster, and have the fields from the flightdelaysummary Hive table loaded in the report design surface.

### Task 3: Create Power BI report

#### Tasks to complete:

-   Generate a Power BI report containing Map, Stacked Column Chart, and Treemap visualizations of the flight delay summary data.

-   The Map visualization should represent the number of delays, based on the location of the airport.

-   The Stacked Column Chart should provide information about the probability of a delay, based on the day.

-   The Treemap visual display details about the number of delays associated with a particular airport.

#### Exit criteria:

-   You should have a Power BI report generated, contain three interlinked tiles, displaying flight delay details.

    ![The Power BI report has a Map visualization, Stacked column chart, and treemap visualization.](images/Hands-onlabunguided-Bigdataandvisualizationimages/media/image37.png "Power BI report")

## Exercise 7: Deploy intelligent web app

Duration: 20 minutes

In this exercise, you will deploy an intelligent web application to Azure from GitHub. This application leverages the operationalized machine learning model that was deployed in Exercise 1 to bring action-oriented insight to an already existing business process.

### Task 1: Deploy web app from GitHub

#### Tasks to complete:

-   Navigate to the AdventureWorks README page (<https://github.com/ZoinerTejada/mcw-big-data-and-visualization/blob/master/AdventureWorksTravel/README.md>), and deploy a web app to Azure using an ARM template.

-   Provide your ML API key, and service details, which can be retrieved from <https://services.azureml.net>, and looking at your web service.

-   Enter your Weather Underground API key as part of the deployment process.

#### Exit criteria:

-   You are able to successfully navigate to the deployed web app, and test various airport connections to view weather and delay prediction details.

    ![The AdventureWorks Travel webpage displays.](images/Hands-onlabunguided-Bigdataandvisualizationimages/media/image38.png "AdventureWorks Travel webpage")

## After the hands-on lab 

Duration: 10 minutes

In this exercise, attendees will deprovision any Azure resources that were created in support of the lab.

### Task 1: Delete resource group

1.  Using the Azure portal, navigate to the Resource group you used throughout this hands-on lab by selecting **Resource groups** in the left menu.

2.  Search for the name of your research group and select it from the list.

3.  Select **Delete** in the command bar and confirm the deletion by re-typing the Resource group name and selecting **Delete**.

You should follow all steps provided *after* attending the Hands-on lab.

