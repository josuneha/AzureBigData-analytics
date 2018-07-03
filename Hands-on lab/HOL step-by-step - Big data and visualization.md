![](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png 'Microsoft Cloud Workshops')

<div class="MCWHeader1">
Big data and visualization
</div>
    
<div class="MCWHeader2">
Hands-on lab step-by-step
</div>

<div class="MCWHeader3">
May 2018
</div>

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

# Big data and visualization hands-on lab step-by-step

**Contents**

<!-- TOC -->

- [Big data and visualization](#big-data-and-visualization)
  - [Hands-on lab step-by-step](#hands-on-lab-step-by-step)
- [Big data and visualization hands-on lab step-by-step](#big-data-and-visualization-hands-on-lab-step-by-step)
  - [Abstract and learning objectives](#abstract-and-learning-objectives)
  - [Overview](#overview)
  - [Solution architecture](#solution-architecture)
  - [Requirements](#requirements)
  - [Exercise 1: Build a Machine Learning Model](#exercise-1-build-a-machine-learning-model)
    - [Task 1: Create your Azure Machine Learning project](#task-1-create-your-azure-machine-learning-project)
    - [Task 2: Upload the Sample Datasets](#task-2-upload-the-sample-datasets)
    - [Task 3: Open Azure Databricks and complete lab notebooks](#task-3-open-azure-databricks-and-complete-lab-notebooks)
    - [Task 4: Configure your Machine Learning environment](#task-4-configure-your-machine-learning-environment)
  - [Exercise 2: Setup Azure Data Factory](#exercise-2-setup-azure-data-factory)
    - [Task 1: Download and stage data to be processed](#task-1-download-and-stage-data-to-be-processed)
    - [Task 2: Install and configure Azure Data Factory Integration Runtime on the Lab VM](#task-2-install-and-configure-azure-data-factory-integration-runtime-on-the-lab-vm)
    - [Task 3: Configure Azure Data Factory](#task-3-configure-azure-data-factory)
  - [Exercise 3: Deploy your machine learning model with Azure ML](#exercise-3-deploy-your-machine-learning-model-with-azure-ml)
    - [Task 1: Edit the scoring and configuration files](#task-1-edit-the-scoring-and-configuration-files)
    - [Task 2: Deploy the model](#task-2-deploy-the-model)
  - [Exercise 4: Develop a data factory pipeline for data movement](#exercise-4-develop-a-data-factory-pipeline-for-data-movement)
    - [Task 1: Create copy pipeline using the Copy Data Wizard](#task-1-create-copy-pipeline-using-the-copy-data-wizard)
  - [Exercise 5: Operationalize ML scoring with Azure Databricks and Data Factory](#exercise-5-operationalize-ml-scoring-with-azure-databricks-and-data-factory)
    - [Task 1: Create Azure Databricks Linked Service](#task-1-create-azure-databricks-linked-service)
    - [Task 2: Complete the BatchScore Azure Databricks notebook code](#task-2-complete-the-batchscore-azure-databricks-notebook-code)
    - [Task 3: Trigger workflow](#task-3-trigger-workflow)
  - [Exercise 6: Summarize data using Azure Databricks](#exercise-6-summarize-data-using-azure-databricks)
    - [Task 1: Summarize delays by airport](#task-1-summarize-delays-by-airport)
  - [Exercise 7: Visualizing in Power BI Desktop](#exercise-7-visualizing-in-power-bi-desktop)
    - [Task 1: Obtain the JDBC connection string to your Azure Databricks cluster](#task-1-obtain-the-jdbc-connection-string-to-your-azure-databricks-cluster)
    - [Task 2: Connect to Azure Databricks using Power BI Desktop](#task-2-connect-to-azure-databricks-using-power-bi-desktop)
    - [Task 3: Create Power BI report](#task-3-create-power-bi-report)
  - [Exercise 8: Deploy intelligent web app](#exercise-8-deploy-intelligent-web-app)
    - [Task 1: Deploy web app from GitHub](#task-1-deploy-web-app-from-github)
  - [After the hands-on lab](#after-the-hands-on-lab)
    - [Task 1: Delete resource group](#task-1-delete-resource-group)

<!-- /TOC -->

## Abstract and learning objectives

This hands-on lab is designed to provide exposure to many of Microsoft's transformative line of business applications built using Microsoft big data and advanced analytics. The goal is to show an end-to-end solution, leveraging many of these technologies, but not necessarily doing work in every component possible.

AdventureWorks Travel (AWT) provides concierge services for business travelers. In an increasingly crowded market, they are always looking for ways to differentiate themselves, and provide added value to their corporate customers.

They are looking to pilot a web app that their internal customer service agents can use to provide additional information useful to the traveler during the flight booking process. They want to enable their agents to enter in the flight information and produce a prediction as to whether the departing flight will encounter a 15-minute or longer delay, considering the weather forecasted for the departure hour.

In this workshop, you will deploy a web app using Machine Learning (ML) to predict travel delays given flight delay data and weather conditions. Plan a bulk data import operation, followed by preparation, such as cleaning and manipulating the data for testing, and training your Machine Learning model.

## Overview

AdventureWorks Travel (AWT) provides concierge services for business travelers. In an increasingly crowded market, they are always looking for ways to differentiate themselves, and provide added value to their corporate customers.

They are looking to pilot a web app that their internal customer service agents can use to provide additional information useful to the traveler during the flight booking process. They want to enable their agents to enter in the flight information and produce a prediction as to whether the departing flight will encounter a 15-minute or longer delay, considering the weather forecasted for the departure hour.

## Solution architecture

Below is a diagram of the solution architecture you will build in this lab. Please study this carefully so you understand the whole of the solution as you are working on the various components.

![This is the high-level overview diagram of the end-to-end solution.](../Whiteboard%20design%20session/media/high-level-overview.png 'High-level overview diagram')

## Requirements

1.  Microsoft Azure subscription must be pay-as-you-go or MSDN

    a. Trial subscriptions will not work

1.  Follow all the steps provided in [Before the Hands-on Lab](Before%20the%20HOL%20-%20Big%20data%20and%20visualization.md)

## Exercise 1: Build a Machine Learning Model

Duration: 60 minutes

In this exercise, you will implement a classification experiment. You will load the training data from your local machine into a dataset. Then, you will explore the data to identify the primary components you should use for prediction, and use two different algorithms for predicting the classification. You will then evaluate the performance of both and algorithms choose the algorithm that performs best. The model selected will be exposed as a web service that is integrated with the sample web app.

### Task 1: Create your Azure Machine Learning project

1.  Connect to the Lab VM (DSVM) (If you are already connected to your DSVM, skip to Step 8)

2.  From the side menu in the Azure portal, select **Resource groups**, then enter your resource group name into the filter box, and select it from the list

3.  Next, select your lab Data Science Virtual Machine (DSVM) from the list

    ![Select the Lab DSVM from within your lab resource group](media/select-lab-dsvm.png 'Select Lab DSVM from resource group')

4.  On your Lab DSVM blade, select **Connect** from the top menu

    ![The Connect button is selected on the Lab DSVM blade menu bar.](media/lab-dsvm-connect.png 'Connect button')

5.  In the dialog that appears, accept the defaults and select **Download RDP File**. Open the file once downloaded.

    ![Select Download RDP File](media/lab-dsvm-download-rdp-file.png 'Select download RDP File')

6.  Select Connect, and enter the following credentials (or the non-default credentials if you changed them):

    - User name: demouser

    - Password: Password.1!!

7.  **If you cannot Remote Desktop into the DSVM** due to the following error, **"CredSSP encryption oracle remediation"**, do one of the following:

    - Option 1: Follow this link to workaround the issue: <https://support.microsoft.com/en-us/help/4295591/credssp-encryption-oracle-remediation-error-when-to-rdp-to-azure-vm>

    - Option 2: Install the [Microsoft Remote Desktop app](https://www.microsoft.com/store/productId/9WZDNCRFJ3PS) from the Microsoft Store. The CredSSP issue appears to only affect the Remote Desktop Connection client installed with Windows.

1)  From your Lab VM (DSVM), launch the _Azure Machine Learning Workbench_. You should see a desktop icon for the application, or find it under the Start menu.

2)  You will be prompted to log in with your Azure account. Use the same Azure account you used to provision the services in the [Before the hands-on lab](./Before%20the%20HOL%20-%20Big%20data%20and%20visualization.md) setup.

3)  Once authenticated, you should see the Machine Learning Experimentation Workspace you provisioned, displayed on the welcome page of the Workbench

4)  Select the **+** symbol next to the PROJECTS header above your Experimentation Workspace (1), then select **New Project**. This opens the New Project form. Within the form, enter **FlightDelays** for the project name (2), **C:\HOL** for the project directory (3), ensure your Experimentation workspace is selected, select the **Blank Project** project template (4), then select **Create** (5).

    ![Create new project in the Azure Machine Learning Workbench](media/create-new-workbench-project.png)

5)  This will create the following new file path with a default project structure: C:\HOL\FlightDelays

    ![Project structure generated after creating new Workbench project](media/new-project-structure.png)

### Task 2: Upload the Sample Datasets

1.  Before you begin creating a machine learning experiment, there are three datasets you need to load

2.  Download the three CSV sample datasets from here: <http://bit.ly/2wGAqrl> (If you get an error, or the page won't open, try pasting the URL into a new browser window and verify the case sensitive URL is exactly as shown)

3.  Extract the ZIP and verify you have the following files:

- FlightDelaysWithAirportCodes.csv

- FlightWeatherWithAirportCodes.csv

- AirportCodeLocationLookupClean.csv

4.  From your Lab VM (DSVM), open a browser and navigate to the Azure portal (<https://portal.azure.com>), and navigate to Azure Databricks service under the Resource Group you created when completing the prerequisites for this hands-on lab

    ![Select the Azure Databricks service from within your lab resource group](media/select-azure-databricks-service.png 'Select Azure Databricks')

5.  In the Overview pane of the Azure Databricks service, select **Launch Workspace**

    ![Select Launch Workspace within the Azure Databricks service overview pane](media/azure-databricks-launch-workspace.png 'Select Launch Workspace')

    Azure Databricks will automatically log you in using Azure Active Directory Single Sign On.

    ![Azure Databricks Azure Active Directory Single Sign On](media/azure-databricks-aad.png 'AAD Single Sign On')

6.  Once signed in, select **Data** from the menu. Next, select **default** under Databases (if this does not appear, start your cluster). Finally, select **+** next to the Tables header.

    ![From the Azure Databricks workspace, select Data, default database, then new table](media/azure-databricks-create-tables.png 'Create new table')

7.  Select **Upload File** under Create New Table, and then select either select or drag-and-drop the FlightDelaysWithAirportCodes.csv file into the file area. Select **Create Table with UI**.

    ![Create a new table using the FlightDelaysWithAirportCodes.csv file](<media/![](media/create-flight-delays-table-ui.png).png> 'Create new table')

8.  Select your cluster to preview the table, then select **Preview Table**

9.  Change the Table Name to \"flight_delays_with_airport_codes\" and select the checkmark for **First row is header**. Select **Create Table**.

    ![Rename table and check the first row is header checkbox](media/flight-delays-attributes.png 'Rename table')

10. Repeat the previous steps for the FlightWeatherWithAirportCode.csv and AirportCodeLocationsClean.csv files, setting the name for each dataset in a similar fashion. There should be a total of three files that are uploaded. Each table should be named as follows:

    - flightweatherwithairportcode_csv to **flight_weather_with_airport_code**
    - flightdelayswithairportcodes_csv to **flight_delays_with_airport_codes**
    - airportcodelocationlookupclean_csv to **airport_code_location_lookup_clean**

    ![Azure Databricks tables shown after all three files uploaded](media/uploaded-data-files.png 'Uploaded data files')

### Task 3: Open Azure Databricks and complete lab notebooks

1.  Download the following files:

    - [01 Prepare Flight Data complete.dbc](lab-files/01%20Prepare%20Flight%20Data%20complete.dbc)
    - [02 Predict Flight Delays complete.dbc](lab-files/02%20Predict%20Flight%20Delays%20complete.dbc)

2.  Within Azure Databricks, select **Workspace** on the menu, then **Users**, select your user, then select the down arrow on the top of your user workspace. Select **Import**.

    ![Screenshot showing selecting import within the user workspace](media/select-import-in-user-workspace.png 'Import')

3.  Within the Import Notebooks dialog, select Import from: file, then drag-and-drop the files or browse to upload each individually

    ![Select import from file](media/import-notebooks.png 'Import from file')

4.  Select **01 Prepare Flight Data complete** to open the notebook

5.  Before you begin, make sure you attach your cluster to the notebook, using the dropdown. You will need to do this for each notebook you open.

    ![Select your cluster to attach it to the notebook](media/attach-cluster-to-notebook.png 'Attach cluster to notebook')

6.  Run each cell of the notebook individually by selecting within the cell, then entering **Ctrl+Shift** on your keyboard. Pay close attention to the instructions within the notebook so you understand each step of the data preparation process.

7.  Repeat the process for **02 Predict Flight Delays complete**

### Task 4: Configure your Machine Learning environment

1.  From your Lab VM (DSVM), browse to the download folder in File Explorer. Right-click **flight_delays.zip** (which was downloaded at the end of the **02 Predict Flight Delays complete** notebook), then select **Extract All...**.

    ![Right-click downloaded flight_delays.zip file and select Extract All from the context menu](media/downloaded-flight-delays-zip.png 'Extract all')

2.  In the extract dialog, accept the default options and select **Extract**

    ![Extract dialog showing default options](media/extract-flight-delays-dialog.png 'Extract dialog')

3.  Within the extracted folder, navigate to dbfs\tmp\models and copy the **pipelineModel** subfolder

4.  Navigate to your project folder (C:\HOL\FlightDelays) and paste the pipelineModel subfolder within

    ![Copy pipelineModel to the FlightDelays project folder](media/model-copied-to-project-folder.png 'Model copied to project folder')

5.  Open the Azure Machine Learning Workbench. Select File --> Open Command Prompt.

    ![Open command prompt option in the workbench](media/azure-ml-workbench-open-command-prompt.png 'Open command prompt...')

6.  Execute the following to update/install the Azure CLI packages:

    ```bash
    pip install azure-cli azure-cli-ml azure-ml-api-sdk
    ```

7.  Execute the following to upgrade the `pyspark` version:

    ```bash
    pip install pyspark --upgrade
    ```

8.  Set up your machine learning environment with the following command:

    ```bash
    az ml env setup -c -n <environment name> --location <location> --resource-group <yourresourcegroupname>
    ```

        Replace the tokens above with appropriate values:

        - For <environment name> enter flightdelays, or something similar. This value can only contain lowercase alphanumeric characters.
        - For <location>, use eastus2, westcentralus, australiaeast, westeurope, or southeastasia, as those are the only acceptable values at this time.
        - For <yourresourcegroupname>, enter the resource group name you've been using for this lab.

9.  If prompted, copy the URL presented and sign in using your web browser

10. Enter the code provided in the command prompt

11. Return to the command prompt, which should automatically update after you log in

12. At the "Subscription set to <subscription name>" prompt, enter Y if the subscription name is correct, or N to select the subscription to use from a list

13. It will take **10-20 minutes** for your ACS cluster to be ready. You can periodically check on the status by running the command shown in the output to the previous step, which is of the form:

    ```bash
    az ml env show -g <resourceGroupName> -n <clusterName>
    ```

## Exercise 2: Setup Azure Data Factory

Duration: 20 minutes

In this exercise, you will create a baseline environment for Azure Data Factory development for further operationalization of data movement and processing. You will create a Data Factory service, and then install the Data Management Gateway which is the agent that facilitates data movement from on-premises to Microsoft Azure.

### Task 1: Download and stage data to be processed

1.  Sign into the Lab VM (DSVM) and open a web browser

2.  Download the AdventureWorks sample data from <http://bit.ly/2zi4Sqa>

3.  Extract it to a new folder called **C:\\Data**

### Task 2: Install and configure Azure Data Factory Integration Runtime on the Lab VM

1.  To download the latest version of Azure Data Factory Integration Runtime, go to <https://www.microsoft.com/en-us/download/details.aspx?id=39717>

    ![The Azure Data Factory Integration Runtime Download webpage displays.](media/image112.png 'Azure Data Factory Integration Runtime Download webpage')

2.  Select Download, then choose the download you want from the next screen

    ![Under Choose the download you want, IntegrationRuntime_3.0.6464.2 (64-bit).msi is selected.](media/image113.png 'Choose the download you want section')

3.  Run the installer, once downloaded

4.  When you see the following screen, select Next

    ![The Welcome page in the Microsoft Integration Runtime Setup Wizard displays.](media/image114.png 'Microsoft Integration Runtime Setup Wizard')

5.  Check the box to accept the terms and select Next

    ![On the End-User License Agreement page, the check box to accept the license agreement is selected, as is the Next button.](media/image115.png 'End-User License Agreement page')

6.  Accept the default Destination Folder, and select Next

    ![On the Destination folder page, the destination folder is set to C;\Program Files\Microsoft Integration Runtime\ and the Next button is selected.](media/image116.png 'Destination folder page')

7.  Select Install to complete the installation

    ![On the Ready to install Microsoft Integration Runtime page, the Install button is selected.](media/image117.png 'Ready to install page')

8.  Select Finish once the installation has completed

    ![On the Completed the Microsoft Integration Runtime Setup Wizard page, the Finish button is selected.](media/image118.png 'Completed the Wizard page')

9.  After selecting Finish, the following screen will appear. Keep it open for now. You will come back to this screen once the Data Factory in Azure has been provisioned, and obtain the gateway key so we can connect Data Factory to this "on-premises" server.

    ![The Microsoft Integration Runtime Configuration Manager, Register Integration Runtime page displays.](media/image119.png 'Register Integration Runtime page')

### Task 3: Configure Azure Data Factory

1.  Launch a new browser window, and navigate to the Azure portal (<https://portal.azure.com>). Once prompted, log in with your Microsoft Azure credentials. If prompted, choose whether your account is an organization account or a Microsoft account. This will be based on which account was used to provision your Azure subscription that is being used for this lab.

2.  From the side menu in the Azure portal, choose **Resource groups**, then enter your resource group name into the filter box, and select it from the list

3.  Next, select your Azure Data Factory service from the list

4.  On the Data Factory blade, select **Author & Monitor** under Actions

    ![In the Azure Data Factory blade, under Actions, the Author & Monitor option is selected.](media/adf-author-monitor.png 'Author & Monitor')

5.  A new page will open in another tab or new window. Within the Azure Data Factory site, select **Author** (the pencil icon) on the menu

    ![Select Author from the menu](media/adf-home-author-link.png 'Author link on ADF home page')

6.  Now, select **Connections** at the bottom of Factory Resources (1), then select the **Integration Runtimes** tab (2), and finally select **+ New** (3)

    ![Select Connections at the bottom of the page, then select the Integration Runtimes tab, and select New.](media/adf-new-ir.png 'Steps to create a new Integation Runtime connection')

7.  In the Integration Runtime Setup blade that appears, select "Perform data movement and dispatch activities to external computes", then select **Next**

    ![Select Perform data movement and dispatch activities to external computes](media/adf-ir-setup-1.png 'Integration Runtime Setup step 1')

8.  Select **Private Network** then select **Next**

    ![Select Private Network then Next](media/adf-ir-setup-2.png 'Integration Runtime Setup step 2')

9.  Enter a **Name**, such as bigdatagateway-\[initials\], and select **Next**

    ![Enter a Name and select Next](media/adf-ir-setup-3.png 'Integration Runtime Setup step 3')

10. Under Option 2: Manual setup, copy the Key1 authentication key value by selecting the Copy button, then select **Finish**


    ![Copy the Key1 value](media/adf-ir-setup-4.png 'Integration Runtime Setup step 4')

11. _Don't close the current screen or browser session_

12. Go back to the Remote Desktop session of the **Lab VM**

13. Paste the **Key1** value into the box in the middle of the Microsoft Integration Runtime Configuration Manager screen


    ![The Microsoft Integration Runtime Configuration Manager Register Integration Runtime page displays.](media/image127.png 'Microsoft Integration Runtime Configuration Manager')

14. Select **Register**

15. It will take a minute or two to register. If it takes more than a couple of minutes, and the screen does not respond or returns an error message, close the screen by selecting the **Cancel** button.

16. The next screen will be New Integration Runtime (Self-hosted) Node. Select Finish.


    ![The Microsoft Integration Runtime Configuration Manager New Integration Runtime (Self-hosted) Node page displays.](media/adf-ir-self-hosted-node.png 'Microsoft Integration Runtime Configuration Manager')

17. You will then get a screen with a confirmation message. Select the **Launch Configuration Manager** button to view the connection details.


    ![The Microsoft Integration Runtime Configuration Manager Node is connected to the cloud service page displays with connection details.](media/adf-ir-launch-config-manager.png 'Microsoft Integration Runtime Configuration Manager')

    ![The Microsoft Integration Runtime Configuration Manager details](media/adf-ir-config-manager.png 'Microsoft Integration Runtime Configuration Manager')

18. You can now return to the Azure portal, and view the Integration Runtime you just configured


    ![You can view your Integration Runtime you just configured](media/adf-ir-running.png 'Integration Runtime in running state')

19. Select the Azure Data Factory Overview button on the menu. Leave this open for the next exercise


    ![Select the Azure Data Factory Overview button on the menu](media/adf-overview.png 'ADF Overview')

## Exercise 3: Deploy your machine learning model with Azure ML

Duration: 20 minutes

In this exercise, you will deploy your trained machine learning model to Azure Container Services with the help of Azure Machine Learning Model Management.

### Task 1: Edit the scoring and configuration files

1.  Open the Azure Machine Learning Workbench. Select File --> Open Command Prompt.

    ![Open command prompt option in the workbench](media/azure-ml-workbench-open-command-prompt.png)

2.  Execute the following to update/install the Azure CLI packages:

    ```bash
    pip install azure-cli azure-cli-ml azure-ml-api-sdk
    ```

3.  Execute the following to upgrade the `pyspark` version:

    ```bash
    pip install pyspark --upgrade
    ```

4.  After the package installation is complete, execute the following command from the command prompt to launch a local instance of Jupyter notebooks:

    ```bash
    jupyter notebook
    ```

5.  This command will launch Jupyter in a new web browser. If prompted, select **Firefox** as your default web browser.

    ![Local instance of Jupyter running within the web browser](media/jupyter-in-browser.png)

6.  Select **New**, then the **Python 3 Spark - local** kernel to create a new Jupyter notebook

    ![Screenshot showing UI options to create a new Python 3 Spark - local kernel-based notebook](media/new-jupyter-notebook.png 'Jupyter UI to create new notebook')

7.  Now we want to create the same methods we will use in our score.py file to test them out locally. Paste the following code into the first cell:

    ```python
    def init():
    # read in the model file
    from pyspark.ml import Pipeline, PipelineModel
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, Bucketizer
    global pipeline

    pipeline = PipelineModel.load('pipelineModel')

    def generate_api_schema():
        from azureml.api.schema.dataTypes import DataTypes
        from azureml.api.schema.sampleDefinition import SampleDefinition
        from azureml.api.realtime.services import generate_schema
        import os
        print("create schema")
        sample_input = "{\"OriginAirportCode\":\"SAT\",\"Month\":5,\"DayofMonth\":5,\"CRSDepHour\":13,\"DayOfWeek\":7,\"Carrier\":\"MQ\",\"DestAirportCode\":\"ORD\",\"WindSpeed\":9,\"SeaLevelPressure\":30.03,\"HourlyPrecip\":0}"
        inputs = {"input_df": SampleDefinition(DataTypes.STANDARD, sample_input)}
        os.makedirs('outputs', exist_ok=True)
        print(generate_schema(inputs=inputs, filepath="outputs/schema.json", run_func=run))

    def run(input_df):
        from pyspark.context import SparkContext
        from pyspark.sql.session import SparkSession
        sc = SparkContext.getOrCreate()
        spark = SparkSession(sc)
        response = ''

        try:
            import json
            # Set inferSchema=true to prevent the float values from being seen as strings
            # which can later cause the VectorAssembler to throw an error: 'Data type StringType is not supported.'
            df = spark.read.option("inferSchema", "true").json(sc.parallelize([input_df]))

            #Get prediction results for the dataframe
            score = pipeline.transform(df)
            predictions = score.collect()
            #response = df
            #Get each scored result
            for pred in predictions:
                confidence = str(pred['probability'][0]) if pred['prediction'] == 0 else str(pred['probability'][1])
                response += "{\"prediction\":" + str(pred['prediction']) + ",\"probability\":" +  confidence + "},"
            # Remove the last comma
            response = response[:-1]
        except Exception as e:
            return (str(e))

        # Return results
        return response
    ```

8.  Run this cell and create a new one beneath it. The keyboard shortcut is `Shift+Enter`.

9.  Paste the following into the second cell:

    ```python
    testInput = "{\"OriginAirportCode\":\"SAT\",\"Month\":5,\"DayofMonth\":5,\"CRSDepHour\":13,\"DayOfWeek\":7,\"Carrier\":\"MQ\",\"DestAirportCode\":\"ORD\",\"WindSpeed\":9,\"SeaLevelPressure\":30.03,\"HourlyPrecip\":0}"
    testInput2 = "{\"OriginAirportCode\":\"ATL\",\"Month\":2,\"DayofMonth\":5,\"CRSDepHour\":8,\"DayOfWeek\":4,\"Carrier\":\"MQ\",\"DestAirportCode\":\"MCO\",\"WindSpeed\":3,\"SeaLevelPressure\":31.03,\"HourlyPrecip\":0}"
    # test init() in local notebook# test  
    init()

    # test run() in local notebook
    run("[" + testInput + "," + testInput2 + "]")
    ```

10. Run the cell. This tests the `run()` method, passing in two test parameters. One should return a prediction of 1, and the other 0, though your results may vary. Your output should look similar to the following: `'{"prediction":1.0,"probability":0.560342524075},{"prediction":0.0,"probability":0.69909752}'`. If everything works as expected, then we are ready to modify the score.py file. Save and close this notebook to return to the Jupyter home page.

11. Open **score.py**. Replace the file contents with the following:

    ```python
    # This script generates the scoring file
    # necessary to operationalize your model
    from azureml.api.schema.dataTypes import DataTypes
    from pyspark.ml import Pipeline, PipelineModel
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler
    from pyspark.context import SparkContext
    from pyspark.sql.session import SparkSession
    sc = SparkContext.getOrCreate()
    spark = SparkSession(sc)

    # Prepare the web service definition by authoring
    # init() and run() functions. Test the functions
    # before deploying the web service.

    model = None

    def init():
        # read in the model file
        global pipeline
        pipeline = PipelineModel.load("pipelineModel")

    def run(input_df):
        response = ''

        try:
            import json
            # Set inferSchema=true to prevent the float values from being seen as strings
            # which can later cause the VectorAssembler to throw an error: 'Data type StringType is not supported.'
            df = spark.read.option("inferSchema", "true").json(sc.parallelize([input_df]))

            #Get prediction results for the dataframe
            score = pipeline.transform(df)
            predictions = score.collect()
            #response = df
            #Get each scored result
            for pred in predictions:
                confidence = str(pred['probability'][0]) if pred['prediction'] == 0 else str(pred['probability'][1])
                response += "{\"prediction\":" + str(pred['prediction']) + ",\"probability\":" +  confidence + "},"
            # Remove the last comma
            response = response[:-1]
        except Exception as e:
            return (str(e))

        # Return results
        return response
    ```

12. **Save** your changes. Close score.py.

13. From the Jupyter home page, open the **aml_config** folder, then open **conda_dependencies.yml**

    ![Open the aml_config/conda_dependencies.yml file](media/jupyter-conda-dependencies.png 'Jupyter UI with file list')

14. Replace the file contents with the following:

    ```yml
    # Conda environment specification. The dependencies defined in this file will
    # be automatically provisioned for managed runs. These include runs against
    # the localdocker, remotedocker, and cluster compute targets.

    # Note that this file is NOT used to automatically manage dependencies for the
    # local compute target. To provision these dependencies locally, run:
    # conda env update --file conda_dependencies.yml

    # Details about the Conda environment file format:
    # https://conda.io/docs/using/envs.html#create-environment-file-by-hand

    # For managing Spark packages and configuration, see spark_dependencies.yml.

    # Version of this configuration file's structure and semantics in AzureML.
    # This directive is stored in a comment to preserve the Conda file structure.
    # [AzureMlVersion] = 2

    name: project_environment
    dependencies:
      # The python interpreter version.
      # Currently Azure ML Workbench only supports 3.5.2.
      - python=3.5.2

      # Required for Jupyter Notebooks.
      - ipykernel=4.6.1

      - pip:
        # Required packages for AzureML execution, history, and data preparation.
        - --index-url https://azuremldownloads.azureedge.net/python-repository/preview
        - --extra-index-url https://pypi.python.org/simple
        - azureml-requirements

        # The API for Azure Machine Learning Model Management Service.
        # Details: https://github.com/Azure/Machine-Learning-Operationalization
        - azure-ml-api-sdk==0.1.0a11
        - azureml.datacollector==0.1.0a13
        - pyspark
    ```

15. **Save** your changes. Close conda_dependencies.yml.

### Task 2: Deploy the model

1.  Open a new Command Prompt from the Azure Machine Learning Workbench

2.  Associate your Model Management account, using the account name you created during setup when you provisioned the Machine Learning Experimentation service. Also be sure to use your same resource group you've been using throughout the lab:

    ```bash
    az ml account modelmanagement set -n <youracctname> -g <yourresourcegroupname>
    ```

    ![Associate model management account command](media/associate-model-management.png)

3.  Set the environment. This sets the context for the remaining commands. The <environment name> is the value used in Exercise 1, Task 4, Step 8 above. The <yourresourcegroupname> value should be your lab resource group.

    ```bash
    az ml env set -n <yourclustername> -g <yourresourcegroupname>
    ```

    ![The command to set the environment](media/aml-set-environment.png 'az ml env set command')

4.  Now it's time to deploy your web service. This command assumes you are located within your project directory (C:\HOL\FlightDelays) Execute the following command:

    ```bash
    az ml service create realtime --model-file pipelineModel -f score.py -n pred -c aml_config\conda_dependencies.yml -r spark-py
    ```

5.  After the command has successfully run, you will see sample usage as well as a command for additional usage information in the format of: `az ml service usage realtime -i pred.[flightdelays-xyz.location]`

    ![Screenshot showing command prompt with az ml service create realtime command](media/aml-create-realtime.png 'az ml service create realtime command')

    Also, this is a good time to test your service: `az ml service run realtime -i pred.[flightdelays-xyz.location] -d "{\"OriginAirportCode\":\"SAT\",\"Month\":5,\"DayofMonth\":5,\"CRSDepHour\":13,\"DayOfWeek\":7,\"Carrier\":\"MQ\",\"DestAirportCode\":\"ORD\",\"WindSpeed\":9,\"SeaLevelPressure\":30.03,\"HourlyPrecip\":0}"`

    This should have an output like `{"prediction":1.0,"probability":0.560342524075}`

6.  View additional usage to see your Scoring URL and how to pass authentication: `az ml service usage realtime -i pred.[flightdelays-xyz.location]`. **Copy the Scoring URL** to Notepad or similar for later reference.

    ![Additional usage information - copy the Scoring URL](media/aml-more-info.png 'az ml service usage realtime command')

7.  Copy the command for key generation and execute. The format is: `az ml service keys realtime -i pred.[flightdelays-xyz.location]`. **Copy the PrimaryKey value** to Notepad or similar for later reference.

    ![Copy the PrimaryKey value for later reference](media/aml-service-keys.png 'az ml service keys realtime command')

## Exercise 4: Develop a data factory pipeline for data movement

Duration: 20 minutes

In this exercise, you will create an Azure Data Factory pipeline to copy data (.CSV files) from an on-premises server (Lab VM) to Azure Blob Storage. The goal of the exercise is to demonstrate data movement from an on-premises location to Azure Storage (via the Integration Runtime).

### Task 1: Create copy pipeline using the Copy Data Wizard

1.  Within the Azure Data Factory overview page, select **Copy Data**

    ![Select Copy Data from the overview page](media/adf-copy-data-link.png 'Copy Data')

2.  In the Copy Data properties, enter the following:

    - Task name: **CopyOnPrem2AzurePipeline**

    - Task description: (Optional) **"This pipeline copies timesliced CSV files from on-premises virtual machine C:\\Data to Azure Blob Storage as a continuous job"**

    - Task cadence or Task schedule: **Select Run regularly on schedule**

    - Trigger type: **Select Schedule**

    - Start date time (UTC): **03/01/2017 12:00 am**

    - Recurrence: **Select Monthly, and every 1 month**

    - End: **No End**

    ![Set the ADF pipeline copy activity properties by setting the Task Name to CopyOnPrem2AzurePipeline, adding a description, setting the Task cadence to Run regularly on a Monthly schedule, every 1 month.](media/adf-copy-data-properties.png 'Properties dialog box')

3.  Select **Next**

4.  On the Source data store screen, select **+ Create new connection**

5.  Scroll through the options and select **File System**, then select **Continue**

    ![Select File System, then Continue](media/adf-copy-data-new-linked-service.png 'Select File System')

6.  In the New Linked Service form, enter the following:

    - Name: **OnPremServer**

    - Connect via integration runtime: **Select the Integration runtime created previously in this exercise**

    - Host: **C:\\Data**

    - User name: Enter **demouser**

    - Password: Enter **Password.1!!**

7.  Select **Test connection** to verify you correctly entered the values. Finally, select **Finish**

    ![On the Copy Data activity, specify File server share connection page, fields are set to the previously defined values.](media/adf-copy-data-linked-service-settings.png 'New Linked Service settings')

8.  On the Source data store page, select **Next**

    ![Select Next](media/adf-copy-data-source-next.png 'Select Next')

9.  On the **Choose the input file or folder** screen, select **Browse**, then select the **FlightsAndWeather** folder. Next, check **Copy file recursively**, then select **Next**.

    ![In the Choose the input file or folder section, the FlightsandWeather folder is selected.](media/adf-copy-data-source-choose-input.png 'Choose the input file or folder page')

10. On the File format settings page, select the following options:


    - File format: **Text format**

    - Column delimiter: **Comma (,)**

    - Row delimiter: **Carriage Return + Line feed (\r\n)**

    - Skip line count: **0**

    - Source files contain column names in the first row: **Checked**

    - Treat empty column value as null: **Checked**

    ![Enter the form values](media/adf-copy-data-file-format.png 'File format settings')

11. Select **Next**

12. On the Destination screen, select **+ Create new connection**

13. Select **Azure Blob Storage** within the New Linked Service blade, then select **Continue**


    ![Select Azure Blob Storage, then Continue](media/adf-copy-data-blob-storage.png 'Select Blob Storage')

14. On the New Lined Service (Azure Blob Storage) account screen, enter the following and then choose **Finish**


    - Name: **BlobStorageOutput**

    - Connect via integration runtime: **Select your Integration Runtime**

    - Azure selection method: **From Azure subscription**

    - Storage account name: **Select the blob storage account you provisioned in the before-the-lab section**

    ![On the Copy Data New Linked Service Azure Blob storage account page, fields are set to the previously defined settings.](media/adf-copy-data-blob-storage-linked.png 'New Linked Service Blob Storage')

15. On the Destination data store page, select **Next**

16. Select **Azure Blob Storage** within the Azure storage service dropdown list, then select **Next**


    ![Select Azure Blob Storage](media/adf-copy-data-destination-connection.png 'Connection properties')

17. From the **Choose the output file or folder** tab, enter the following:


    - Folder path: **sparkcontainer/FlightsAndWeather/{Year}/{Month}/**

    - Filename: **FlightsAndWeather.csv**

    - Year: Select **yyyy** from the drop down

    - Month: Select **MM** from the drop down

    - Select **Next**

    - Copy behavior: **Merge files**

      ![On the Copy Data Choose the output file or folder page, fields are set to the previously defined settings.](media/adf-copy-data-output-file-folder.png 'Choose the output file or folder page')

18. On the File format settings screen, select the **Text format** file format, and check the **Add header to file** checkbox, then select **Next**


    ![On the Copy Data File format settings page, the check box for Add header to file is selected.](media/adf-copy-data-file-format-settings.png 'File format settings page')

19. On the **Settings** screen, select **Skip incompatible rows** under Actions. Expand Advanced settings and set Degree of copy parallelism to **10**, then select **Next**.


    ![Select Skip incompatible rows and set copy parallelism to 10](media/adf-copy-data-settings.png 'Settings page')

20. Review settings on the **Summary** tab, but **DO NOT choose Next**


    ![Summary page](media/adf-copy-data-summary.png 'Summary page')

21. Scroll down on the summary page until you see the **Copy Settings** section. Select **Edit** next to **Copy Settings**.


    ![Scroll down and select Edit within Copy Settings](media/adf-copy-data-review-page.png 'Summary page')

22. Change the following Copy settings


    - Retry: Set to **3**

    - Select **Save**

      ![Set retry to 3](media/adf-copy-data-copy-settings.png 'Copy settings')

23. After saving the Copy settings, select **Next** on the Summary tab

24. On the **Deployment** screen you will see a message that the deployment in is progress, and after a minute or two that the deployment completed. Select **Edit Pipeline** to close out of the wizard.


    ![Select Edit Pipeline on the bottom of the page](media/adf-copy-data-deployment.png 'Deployment page')

## Exercise 5: Operationalize ML scoring with Azure Databricks and Data Factory

Duration: 20 minutes

In this exercise, you will extend the Data Factory to operationalize the scoring of data using the previously created machine learning model within an Azure Databricks notebook.

### Task 1: Create Azure Databricks Linked Service

1.  Return to, or reopen, the Author & Monitor page for your Azure Data Factory in a web browser, navigate to the Author view, and select the pipeline

    ![Select the ADF pipeline created in the previous exercise](media/adf-ml-select-pipeline.png 'Select the ADF pipeline')

2.  Once there, expand Databricks under Activities

    ![Expand the Databricks activity after selecting your pipeline](media/adf-ml-expand-databricks-activity.png 'Expand Databricks Activity')

3.  Drag the Notebook activity onto the design surface to the side of the Copy activity

    ![Drag the Notebook onto the design surface](media/adf-ml-drag-notebook-activity.png 'Notebook on design surface')

4.  Select the Notebook activity on the design surface to display tabs containing its properties and settings at the bottom of the screen. On the **General** tab, enter "BatchScore" into the Name field.

    ![Type BatchScore as the Name under the General tab](media/adf-ml-notebook-general.png 'Databricks Notebook General Tab')

5.  Select the **Settings** tab, and select **+ New** next to the Linked service drop down. Here, you will configure a new linked service which will serve as the connection to your Databricks cluster.

    ![Screenshot of the Settings tab](media/adf-ml-settings-new-link.png 'Databricks Notebook Settings Tab')

6.  On the New Linked Service dialog, enter the following:

    - Name: enter a name, such as AzureDatabricks
    - Connect via integration runtime: Leave set to Default
    - Account selection method: Select From Azure subscription
    - Select cluster: choose Existing cluster
    - Domain/Region: select the region where your cluster resides

    ![Screenshot showing filled out form with defined parameters](media/adf-ml-databricks-service-settings.png 'Databricks Linked Service settings')

7.  Leave the form open and open your Azure Databricks workspace in another browser tab. You will retrieve the Access token and cluster id here.

8.  In Azure Databricks, select the Account icon in the top corner of the window, then select **User Settings**

    ![Select account icon, then user settings](media/databricks-select-user-settings.png 'Azure Databricks user account settings')

9.  Select **Generate New Token** under the Access Tokens tab. Enter **ADF access** for the comment and leave the lifetime at 90 days. Select **Generate**.

    ![Generate a new token](media/databricks-generate-new-token.png 'Generate New Token')

10. **Copy** the generated token


    ![Copy the generated token](media/databricks-copy-token.png 'Copy generated token')

11. Switch back to your Azure Data Factory screen and paste the generated token into the **Access token** field within the form


    ![Paste the generated access token](media/adf-ml-access-token.png 'Paste access token')

12. Leave the form open and switch back to Azure Databricks. Select **Clusters** on the menu, then select your cluster in the list. Select the **Tags** tab and copy the **ClusterId** value.


    ![Screenshot of the cluster tags tab](media/databricks-cluster-id.png 'Copy the ClusterId value')

13. Switch back to your Azure Data Factory screen and paste the ClusterId value into the **Existing cluster id** field. Select **Finish**.


    ![Paste the cluster id and select finish](media/adf-ml-databricks-clusterid.png 'Paste cluster id')

14. Switch back to Azure Databricks. Select **Workspace** in the menu. Right-click within Workspace, then select **Create** --> **Folder**.


    ![Right-click within workspace and select Create, Folder](media/databricks-workspace-create-folder.png 'Create folder')

15. Enter **adf** as the folder name, then select **Create Folder**

16. Select the down arrow next to your new folder. Select **Create** --> **Notebook**.


    ![Create a new notebook within the new adf folder](media/databricks-create-notebook.png 'Create notebook')

17. For the name, enter **BatchScore** and make sure Python is selected as the language, and your cluster is selected. Select **Create**.


    ![Enter BatchScore as the new notebook name](media/databricks-create-notebook-form.png 'Create Notebook form')

18. Switch back to your Azure Data Factory screen. Enter **/adf/BatchScore** into the Notebook path field.


    ![Enter /adf/BatchScore into the notebook path](media/adf-ml-notebook-path.png 'Notebook path')

19. The final step is to connect the Copy activities with the Notebook activity. Select the small green box on the side of the copy activity, and drag the arrow onto the Notebook activity on the design surface. What this means is that the copy activity has to complete processing and generate its files in your storage account before the Notebook activity runs, ensuring the files required by the BatchScore notebook are in place at the time of execution. Select **Publish All** after making the connection.


    ![Attach the copy activity to the notebook and then publish](media/adf-ml-connect-copy-to-notebook.png 'Attach the copy activity to the notebook')

### Task 2: Complete the BatchScore Azure Databricks notebook code

We need to complete the notebook code for the batch scoring. For simplicity, we will persist the values in a new global persistent Databricks table. In production data workloads, you may save the scored data to Blob Storage, Azure Cosmos DB, or other serving layer. Another implementation detail we are skipping for the lab is processing only new files. This can be accomplished by creating a widget in the notebook that accepts a path parameter that is passed in from Azure Data Factory.

1.  Switch back to Azure Databricks and open the BatchScore notebook you created within the new `adf` workspace folder

2.  Paste the below into the first cell:

    ```python
    from pyspark.ml import Pipeline, PipelineModel
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, Bucketizer
    from pyspark.sql.functions import array, col, lit
    from pyspark.sql.types import *
    ```

3.  Add a new cell below and paste the following. Replace STORAGE-ACCOUNT-NAME with the name of your storage account. You can find this in the Azure portal by locating the storage account that you created in the lab setup, within your resource group. The container name is set to the default used for this lab. If yours is different, update the containerName variable accordingly.

    ```python
    accountName = "STORAGE-ACCOUNT-NAME"
    containerName = "sparkcontainer"
    ```

4.  Paste the following into a new cell to define the schema for the CSV files:

    ```python
    data_schema = StructType([
            StructField('OriginAirportCode',StringType()),
            StructField('Month', IntegerType()),
            StructField('DayofMonth', IntegerType()),
            StructField('CRSDepHour', IntegerType()),
            StructField('DayOfWeek', IntegerType()),
            StructField('Carrier', StringType()),
            StructField('DestAirportCode', StringType()),
            StructField('DepDel15', IntegerType()),
            StructField('WindSpeed', DoubleType()),
            StructField('SeaLevelPressure', DoubleType()),  
            StructField('HourlyPrecip', DoubleType())])
    ```

5.  Paste the following into a new cell to create a new DataFrame from the CSV files, applying the schema:

    ```python
    dfDelays = spark.read.csv("wasbs://" + containerName + "@" + accountName + ".blob.core.windows.net/FlightsAndWeather/*/*/FlightsAndWeather.csv",
                        schema=data_schema,
                        sep=",",
                        header=True)
    ```

6.  Paste the following into a new cell to load the trained machine learning model you created earlier in the lab:

    ```python
    # Load the saved pipeline model
    model = PipelineModel.load("/dbfs/FileStore/models/pipelineModel")
    ```

7.  Paste the following into a new cell to make a prediction against the loaded data set:

    ```python
    # Make a prediction against the dataset
    prediction = model.transform(dfDelays)
    ```

8.  Paste the following into a new cell to save the scored data into a new global table:

    ```python
    prediction.write.mode("overwrite").saveAsTable("scoredflights")
    ```

### Task 3: Trigger workflow

1.  Switch back to Azure Data Factory. Select your pipeline if it is not already opened.

2.  Select **Trigger**, then **Trigger Now** located above the pipeline design surface

    ![Manually trigger the pipeline](media/adf-ml-trigger-now.png 'Trigger Now')

3.  Enter **3/1/2017** into the windowStart parameter, then select **Finish**

    ![Screenshot showing the Pipeline Run form](media/adf-ml-pipeline-run.png 'Pipeline Run')

4.  Select **Monitor** in the menu. You will be able to see your pipeline activity in progress as well as the status of past runs.

    ![View your pipeline activity](media/adf-ml-monitor.png 'Monitor')

## Exercise 6: Summarize data using Azure Databricks

Duration: 20 minutes

In this exercise, you will prepare a summary of flight delay data using Spark SQL.

### Task 1: Summarize delays by airport

1.  Open your Azure Databricks workspace and navigate to your user folder where your first two lab files are located

2.  Select the down-arrow next to your user account name, then select **Create** --> **Notebook**

    ![Screenshot showing notebook creation](media/databricks-create-user-notebook.png 'Create Notebook')

3.  Enter a name, such as "Explore data" and make sure Python and your cluster are both selected. Select **Create**.

    ![Create Notebook form](media/databricks-create-user-notebook-form.png 'Create Notebook')

4.  Paste the following into the first notebook cell. This will select the scored data generated by the Azure Data Factory pipeline.

    ```python
    %sql
    select * from scoredflights
    ```

5.  Run the cell. You should see a table displayed with the scored data. Scroll all the way to the side. There you will find the `prediction` column containing the flight delay prediction provided by your machine learning model.

    ![scoredflights table with the prediction column](media/databricks-explore-scoredflights.png 'scoredflights table')

6.  Next, you will create a table that summarizes the flight delays data. Instead of containing one row per flight, this new summary table will contain one row per origin airport at a given hour, along with a count of the quantity of anticipated delays. We also join the `airport_code_location_lookup_clean` table you created at the beginning of the lab, so we can extract the airport coordinates. In a new cell below the results of our previous cell, paste the following text, and run the cell.

    ```sql
    %sql
    SELECT  OriginAirportCode, Month, DayofMonth, CRSDepHour, Sum(prediction) NumDelays,
        CONCAT(Latitude, ',', Longitude) OriginLatLong
        FROM scoredflights s
        INNER JOIN airport_code_location_lookup_clean a
        ON s.OriginAirportCode = a.Airport
        WHERE Month = 4
        GROUP BY OriginAirportCode, OriginLatLong, Month, DayofMonth, CRSDepHour
        Having Sum(prediction) > 1
        ORDER BY NumDelays DESC
    ```

7.  Execution of this cell should return a results table like the following

    ![This Results table now has the following columns: OriginAirportCode, OriginLatLong, Month, Day, Hour, and  NumDelays.](media/databricks-results-table.png 'Results table')

8.  Since the summary data looks good, the final step is to save this summary calculation as a table, which we can later query using Power BI (in the next exercise)

9.  To accomplish this, paste the text below into a new cell, and run the cell

    ```python
    summary = spark.sql("SELECT  OriginAirportCode, Month, DayofMonth, CRSDepHour, Sum(prediction) NumDelays,     CONCAT(Latitude, ',', Longitude) OriginLatLong FROM scoredflights s INNER JOIN airport_code_location_lookup_clean a ON s.OriginAirportCode = a.Airport WHERE Month = 4 GROUP BY OriginAirportCode, OriginLatLong, Month, DayofMonth, CRSDepHour  Having Sum(prediction) > 1 ORDER BY NumDelays DESC")
    ```

10. Paste the text below to save the DataFrame as into a global table, then run the cell


    ```python
    summary.write.mode("overwrite").saveAsTable("flight_delays_summary")
    ```

11. Execute the following to verify the table has data:


    ```python
    %sql
    select * from flight_delays_summary
    ```

12. Select various visualizations underneath the displayed grid

## Exercise 7: Visualizing in Power BI Desktop

Duration: 20 minutes

In this exercise, you will create visualizations in Power BI Desktop.

### Task 1: Obtain the JDBC connection string to your Azure Databricks cluster

Before you begin, you must first obtain the JDBC connection string to your Azure Databricks cluster.

1.  In Azure Databricks, go to Clusters and select your cluster

2.  On the cluster edit page, scroll down and select the JDBC/ODBC tab

    ![Select the JDBC/ODBC tab](media/databricks-power-bi-jdbc.png 'JDBC strings')

3.  On the JDBC/ODBC tab, copy and save the JDBC URL

    - Construct the JDBC server address that you will use when you set up your Spark cluster connection in Power BI Desktop

    - Take the JDBC URL that you copied and saved in step 3 and do the following:

    - Replace jdbc:hive2 with https

    - Remove everything in the path between the port number and sql, retaining the components indicated by the boxes in the image below

    ![Select the parts to create the Power BI connection string](media/databricks-power-bi-spark-address-construct.png 'Construct Power BI connection string')

    - In our example, the server address would be:

    <https://eastus.azuredatabricks.net:443/sql/protocolv1/o/1707858429329790/0614-124738-doubt405> or <https://eastus.azuredatabricks.net:443/sql/protocolv1/o/1707858429329790/lab> (if you choose the aliased version)

### Task 2: Connect to Azure Databricks using Power BI Desktop

1.  On your Lab VM, launch Power BI Desktop by double-clicking on the desktop shortcut

2.  When Power BI Desktop opens, you will need to enter your personal information, or Sign in if you already have an account

    ![The Power BI Desktop Welcome page displays.](media/image177.png 'Power BI Desktop Welcome page')

3.  Select Get data on the screen that is displayed next
    ![On the Power BI Desktop Sign in page, in the pane, Get data is selected.](media/image178.png 'Power BI Desktop Sign in page')

4.  Select **Other** from the side, and select **Spark (Beta)** from the list of available data sources

    ![In the pane of the Get Data page, Other is selected. In the pane, Spark (Beta) is selected.](media/pbi-desktop-get-data.png 'Get Data page')

5.  Select **Connect**

6.  You will receive a prompt warning you that the Spark connector is still in preview. Select **Continue**.

    ![A warning reminds you that the app is still under development.](media/image180.png 'Preview connector warning')

7.  On the next screen, you will be prompted for your Spark cluster information

8.  Paste the JDBC connection string you constructed a few steps ago into the **Server** field

9.  Select the **HTTP** protocol

10. Select **DirectQuery** for the Data Connectivity mode, and select **OK**. This option will offload query tasks to the Azure Databricks Spark cluster, providing near-real time querying


    ![Configure your connection to the Spark cluster](media/pbi-desktop-connect-spark.png 'Spark form')

11. Enter your credentials on the next screen as follows


    a. User name: **token**

    b. Password: Create a new personal access token, following the same steps you took when you connected Azure Databricks to Azure Data Factory. **Paste the new token here**.

    ![Enter "token" for the user name and paste user token into the password field](media/pbi-desktop-login.png 'Enter credentials')

12. Select **Connect**

13. In the Navigator dialog, check the box next to **flight_delays_summary**, and select **Load**


    ![In the Navigator dialog box, in the pane under Display Options, the check box for flight_delays_summary is selected. In the pane, the table of flight delays summary information displays.](media/pbi-desktop-select-table-navigator.png 'Navigator dialog box')

14. It will take several minutes for the data to load into the Power BI Desktop client

### Task 3: Create Power BI report

1.  Once the data finishes loading, you will see the fields appear on the far side of the Power BI Desktop client window

    ![Power BI Desktop fields](media/pbi-desktop-fields.png 'Power BI Desktop Fields')

2.  From the Visualizations area, next to Fields, select the Globe icon to add a Map visualization to the report design surface

    ![On the Power BI Desktop Visualizations palette, the globe icon is selected.](media/image187.png 'Power BI Desktop Visualizatoins palette')

3.  With the Map visualization still selected, drag the **OriginLatLong** field to the **Location** field under Visualizations. Then Next, drag the **NumDelays** field to the **Size** field under Visualizations.

    ![In the Fields column, the check boxes for NumDelays and OriginLatLong are selected. An arrow points from OriginLatLong in the Fields column, to OriginLatLong in the Visualization's Location field. A second arrow points from NumDelays in the Fields column, to NumDelays in the Visualization's Size field.](media/pbi-desktop-configure-map-vis.png 'Visualizations and Fields columns')

4.  You should now see a map that looks similar to the following (resize and zoom on your map if necessary):

    ![On the Report design surface, a Map of the United States displays with varying-sized dots over different cities.](media/pbi-desktop-map-vis.png 'Report design surface')

5.  Unselect the Map visualization by selecting the white space next to the map in the report area

6.  From the Visualizations area, select the **Stacked Column Chart** icon to add a bar chart visual to the report's design surface

    ![The stacked column chart icon is selected on the Visualizations palette.](media/image190.png 'Visualizations palette')

7.  With the Stacked Column Chart still selected, drag the **DayofMonth** field and drop it into the **Axis** field located under Visualizations

8.  Next, drag the **NumDelays** field over, and drop it into the **Value** field

    ![In the Fields column, the check boxes for NumDelays and DayofMonth are selected. An arrow points from NumDelays in the Fields column, to NumDelays in the Visualization's Axis field. A second arrow points from DayofMonth in the Fields column, to DayofMonth in the Visualization's Value field.](media/pbi-desktop-configure-stacked-vis.png 'Visualizations and Fields columns')

9.  Grab the corner of the new Stacked Column Chart visual on the report design surface, and drag it out to make it as wide as the bottom of your report design surface. It should look something like the following.

    ![On the Report Design Surface, under the map of the United States with dots, a stacked bar chart displays.](media/pbi-desktop-stacked-vis.png 'Report Design Surface')

10. Unselect the Stacked Column Chart visual by selecting on the white space next to the map on the design surface

11. From the Visualizations area, select the Treemap icon to add this visualization to the report


    ![On the Visualizations palette, the Treemap icon is selected.](media/image193.png 'Visualizations palette')

12. With the Treemap visualization selected, drag the **OriginAirportCode** field into the **Group** field under Visualizations

13. Next, drag the **NumDelays** field over, and drop it into the **Values** field


    ![In the Fields column, the check boxes for NumDelays and OriginAirportcode are selected. An arrow points from NumDelays in the Fields column, to NumDelays in the Visualization's Values field. A second arrow points from OriginAirportcode in the Fields column, to OriginAirportcode in the Visualization's Group field.](media/pbi-desktop-config-treemap-vis.png 'Visualizations and Fields columns')

14. Grab the corner of the Treemap visual on the report design surface, and expand it to fill the area between the map and the side edge of the design surface. The report should now look similar to the following.


    ![The Report design surface now displays the map of the United States with dots, a stacked bar chart, and a Treeview.](media/pbi-desktop-full-report.png 'Report design surface')

15. You can cross filter any of the visualizations on the report by selecting one of the other visuals within the report, as shown below (This may take a few seconds to change, as the data is loaded)


    ![The map on the Report design surface is now zoomed in on the northeast section of the United States, and the only dot on the map is on Chicago. In the Treeview, all cities except ORD are grayed out. In the stacked bar graph, each bar is now divided into a darker and a lighter color, with the darker color representing the airport.](media/pbi-desktop-full-report-filter.png 'Report design surface')

16. You can save the report, by choosing Save from the File menu, and entering a name and location for the file


    ![The Power BI Save as window displays.](media/image197.png 'Power BI Save as window')

## Exercise 8: Deploy intelligent web app

Duration: 20 minutes

In this exercise, you will deploy an intelligent web application to Azure from GitHub. This application leverages the operationalized machine learning model that was deployed in Exercise 1 to bring action-oriented insight to an already existing business process.

### Task 1: Deploy web app from GitHub

1.  Navigate to <https://github.com/Microsoft/MCW-Big-data-and-visualization/blob/master/Hands-on%20lab/lab-files/AdventureWorksTravel/README.md> in your browser of choice, but where you are already authenticated to the Azure portal

2.  Read through the README information on the GitHub page

3.  Select **Deploy to Azure**

    ![Screenshot of the Deploy to Azure button.](media/deploy-to-azure-button.png 'Deploy to Azure button')

4.  On the following page, ensure the fields are populated correctly

    a. Ensure the correct Directory and Subscription are selected

    b. Select the Resource Group that you have been using throughout this lab

    c. Either keep the default Site name, or provide one that is globally unique, and then choose a Site Location

    d. Finally, enter the ML API and Weather API information

    Note: Recall that you recorded the ML API information in the Machine Learning model deployment exercise.

    Note: Also, recall that you obtained the Weather API key back in the prerequisite steps for the lab. Insert that key into the Weather Api Key field.

![Fields on the Deploy to Azure page are populated with the previously copied information.](media/azure-deployment-form.png 'Deploy to Azure page')

5.  Select **Next**, and on the following screen, select **Deploy**

6.  The page should begin deploying your application while showing you a status of what is currently happening

Note: If you run into errors during the deployment that indicate a bad request or unauthorized, verify that the user you are logged into the portal with an account that is either a Service Administrator or a Co-Administrator. You won't have permissions to deploy the website otherwise.

7.  After a short time, the deployment will complete, and you will be presented with a link to your newly deployed web application. CTRL+Click to open it in a new tab.

8.  Try a few different combinations of origin, destination, date, and time in the application. The information you are shown is the result of both the ML API you published, as well as information retrieved from the DarkSky API.

9.  Congratulations! You have built and deployed an intelligent system to Azure.

## After the hands-on lab

Duration: 10 minutes

In this exercise, attendees will deprovision any Azure resources that were created in support of the lab.

### Task 1: Delete resource group

1.  Using the Azure portal, navigate to the Resource group you used throughout this hands-on lab by selecting **Resource groups** in the menu

2.  Search for the name of your research group and select it from the list

3.  Select **Delete** in the command bar and confirm the deletion by re-typing the Resource group name and selecting **Delete**

You should follow all steps provided _after_ attending the Hands-on lab.
