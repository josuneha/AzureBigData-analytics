![Microsoft Cloud Workshop](../media/ms-cloud-workshop.png "Microsoft Cloud Workshop")

[Legal notice](../legal.md)

Updated May 2018

# Big data and visualization hands-on lab step-by-step

AdventureWorks Travel (AWT) provides concierge services for business travelers. In an increasingly crowded market, they are always looking for ways to differentiate themselves, and provide added value to their corporate customers.

They are looking to pilot a web app that their internal customer service agents can use to provide additional information useful to the traveler during the flight booking process. They want to enable their agents to enter in the flight information and produce a prediction as to whether the departing flight will encounter a 15-minute or longer delay, considering the weather forecasted for the departure hour.

In this workshop, you will deploy a web app using Machine Learning (ML) to predict travel delays given flight delay data and weather conditions. Plan a bulk data import operation, followed by preparation, such as cleaning and manipulating the data for testing, and training your Machine Learning model.

If you have not yet completed the steps to set up your environment in [Before the hands-on lab](./Setup.md), you will need to do that before proceeding.

## Contents

<!-- TOC -->

- [Big data and visualization hands-on lab step-by-step](#big-data-and-visualization-hands-on-lab-step-by-step)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Overview](#overview)
    - [Solution architecture](#solution-architecture)
    - [Requirements](#requirements)
    - [Exercise 1: Build a Machine Learning Model](#exercise-1-build-a-machine-learning-model)
        - [Task 1: Navigate to Machine Learning Studio](#task-1-navigate-to-machine-learning-studio)
        - [Task 2: Upload the Sample Datasets](#task-2-upload-the-sample-datasets)
        - [Task 3: Start a new experiment](#task-3-start-a-new-experiment)
        - [Task 4: Prepare flight delay data](#task-4-prepare-flight-delay-data)
        - [Task 5: Prepare the weather data](#task-5-prepare-the-weather-data)
        - [Task 6: Join the Flight and Weather datasets](#task-6-join-the-flight-and-weather-datasets)
        - [Task 7: Train the model](#task-7-train-the-model)
        - [Task 8: Operationalize the experiment](#task-8-operationalize-the-experiment)
    - [Exercise 2: Setup Azure Data Factory](#exercise-2-setup-azure-data-factory)
        - [Task 1: Connect to the Lab VM](#task-1-connect-to-the-lab-vm)
        - [Task 2: Download and stage data to be processed](#task-2-download-and-stage-data-to-be-processed)
        - [Task 3: Install and configure Azure Data Factory Integration Runtime on the Lab VM](#task-3-install-and-configure-azure-data-factory-integration-runtime-on-the-lab-vm)
        - [Task 4: Create an Azure Data Factory](#task-4-create-an-azure-data-factory)
    - [Exercise 3: Develop a data factory pipeline for data movement](#exercise-3-develop-a-data-factory-pipeline-for-data-movement)
        - [Task 1: Create copy pipeline using the Copy Data Wizard](#task-1-create-copy-pipeline-using-the-copy-data-wizard)
    - [Exercise 4: Operationalize ML scoring with Azure ML and Data Factory](#exercise-4-operationalize-ml-scoring-with-azure-ml-and-data-factory)
        - [Task 1: Create Azure ML Linked Service](#task-1-create-azure-ml-linked-service)
        - [Task 2: Create Azure ML input dataset](#task-2-create-azure-ml-input-dataset)
        - [Task 3: Create Azure ML scored dataset](#task-3-create-azure-ml-scored-dataset)
        - [Task 4: Create Azure ML predictive pipeline](#task-4-create-azure-ml-predictive-pipeline)
        - [Task 5: Monitor pipeline activities](#task-5-monitor-pipeline-activities)
    - [Exercise 5: Summarize data using HDInsight Spark](#exercise-5-summarize-data-using-hdinsight-spark)
        - [Task 1: Install pandas on the HDInsight cluster](#task-1-install-pandas-on-the-hdinsight-cluster)
        - [Task 2: Summarize delays by airport](#task-2-summarize-delays-by-airport)
    - [Exercise 6: Visualizing in Power BI Desktop](#exercise-6-visualizing-in-power-bi-desktop)
        - [Task 1: Connect to the Lab VM](#task-1-connect-to-the-lab-vm-1)
        - [Task 2: Connect to HDInsight Spark using Power BI Desktop](#task-2-connect-to-hdinsight-spark-using-power-bi-desktop)
        - [Task 3: Create Power BI report](#task-3-create-power-bi-report)
    - [Exercise 7: Deploy intelligent web app](#exercise-7-deploy-intelligent-web-app)
        - [Task 1: Deploy web app from GitHub](#task-1-deploy-web-app-from-github)
    - [After the hands-on lab](#after-the-hands-on-lab)
        - [Task 1: Delete resource group](#task-1-delete-resource-group)

<!-- /TOC -->

## Exercise 1: Build a Machine Learning Model

Duration: 60 minutes

In this exercise, attendees will implement a classification experiment. They will load the training data from their local machine into a dataset. Then, they will explore the data to identify the primary components they should use for prediction, and use two different algorithms for predicting the classification. They will evaluate the performance of both and algorithms choose the algorithm that performs best. The model selected will be exposed as a web service that is integrated with the sample web app.

### Task 1: Navigate to Machine Learning Studio

1.  From your Lab VM open a browser and navigate to the Azure portal (<https://portal.azure.com>), and navigate to your Machine Learning Studio workspace under the Resource Group you created when completing the prerequisites for this hands-on lab. 

    ![In the Resource group, the kyleml machine learning studio workspace in South Central US is selected.](media/image25.png "Resource group")

2.  On the Machine Learning Studio workspace blade, select **Launch Machine Learning Studio**. ![In the Machine Learning Studio workspace blade, Launch Machine Learning Studio is selected.](media/image26.png "Machine Learning Studio workspace blade")

3.  Sign in.

    ![Sign In link for ML Studio](media/image27.png "Resource Group")

4.  If you have multiple Azure ML workspaces, choose the one you created for this hands-on lab from the drop-down menu near the top right of Azure Machine Learning Studio. ![kyleml is selected from the Azure Machine Learning Studio drop-down menu.](media/image28.png "Azure Machine Learning Studio drop-down menu")

### Task 2: Upload the Sample Datasets

1.  Before you begin creating a machine learning experiment, there are three datasets you need to load.

2.  Download the three CSV sample datasets from here: <http://bit.ly/2wGAqrl> (If you get an error, or the page won't open, try pasting the URL into a new browser window and verify the case sensitive URL is exactly as shown).

3.  Extract the ZIP and verify you have the following files:

-   FlightDelaysWithAirportCodes.csv

-   FlightWeatherWithAirportCodes.csv

-   AirportCodeLocationLookupClean.csv

4.  In the Machine Learning Studio browser window, select **+ NEW** at the bottom left. ![In the Azure Machine Learning Studio browser window, the New button is circled.](media/image29.png "Azure Machine Learning Studio browser window")

5.  Select **Dataset** under New, and then select **From Local File**.
    
    ![Under the New button, Dataset and From Local File are selected.](media/image30.png "New button")

6.  In the dialog that appears, select **Choose File**, browse to the FlightDelaysWithAirportCodes.csv file you downloaded in the previous step, and select **Open**.

    ![In the File Explorer window,the FlightDelaysWithAirportCode.csv file being selected.](media/image31.png "File Explorer")

7.  Change the name of the dataset to \"FlightDelaysWithAirportCodes,\" and select the checkmark to upload the data into a new dataset.

    ![On the Upload a new dataset page, in the Enter a Name for the New Dataset field, FlightDelaysWithAirportCodes is typed. At the bottom, the check mark is selected.](media/image32.png "Upload a new dataset page")

8.  Repeat the previous step for the FlightWeatherWithAirportCode.csv and AirportCodeLocationsClean.csv files, setting the name for each dataset in a similar fashion. There should be a total of three files that are uploaded.

    ![ML Studio is shown after all three files have been uploaded to the Experiments.](media/image33.png "Uploaded data files")

### Task 3: Start a new experiment

1.  Select **+ NEW** in the command bar at the bottom left of the page, and select **Experiment**.

2.  From the options that appear, select **Blank Experiment**. 

    ![In the Machine Learning Studio browser window, under New, Experiment is selected. In the gallery, Blank Experiment displays.](media/image34.png "Machine Learning Studio browser window")

3.  Give your new experiment a name, such as AdventureWorks Travel by editing the \"Experiment created on \...\" label near the top of the design surface. 

    ![In the Machine Learning Studio browser window, AdventureWorks Travel displays in the right pane.](media/image35.png "Machine Learning Studio browser window")

### Task 4: Prepare flight delay data

1.  In the toolbar on the left, in the **Search experiment iteThis will change to a number once the command is complete.\ms** box, type the name of the dataset you created with flight delay data (FlightDelaysWithAirportCodes). You should see a component for it listed under Saved Datasets, My Datasets.

    ![In the Search field, flightdelay is typed. Under My Datasets, FlightDelaysWithAirportCodes is selected.](media/image36.png "Search field")

2.  Select and drag the **FlightDelaysWithAirportCodes** module onto the design surface.

    ![FlightDelaysWithAirportCodes displays on the design surface.](media/image37.png "Design surface")

3.  Next, you will explore the Flight delays datasets to understand what kind of cleanup (e.g., data munging) will be necessary.

4.  Hover over the output port of the **FlightDelaysWithAirportCodes** module. 
    
    ![A callout points to the output port of the FlightDelaysWithAirportCodes module.](media/image38.png "Design surface")

5.  Right-click on the port and select **Visualize**.

    ![Visualize is selected from the FlightDelaysWithAirportCodes output port drop-down menu.](media/image39.png "drop-down menu")

6.  A new dialog will appear showing a maximum of 100 rows by 100 columns sample of the dataset. You can see at the top that the dataset has a total of 2,719,418 rows (also referred to as examples in Machine Learning literature) and has 20 columns (also referred to as features). 

    ![A FlightDelaysWithAirportCodes dataset displays. ](media/image40.png "FlightDelaysWithAirportCodes dataset")

7.  Because all 20 columns are displayed, you can scroll the grid horizontally. Scroll until you see the **DepDel15** column, and select it to view statistics about the column. The DepDel15 column displays a 1 when the flight was delayed at least 15 minutes and 0 if there was no such delay. In the model you will construct, you will try to predict the value of this column for future data. Notice in the Statistics panel that a value of 27444 appears for Missing Values. This means that 27,444 rows do not have a value in this column. Since this value is very important to our model, we will need to eliminate any rows that do not have a value for this column.

    ![In the FlightDelaysWithAirportCodes dataset window, under Statistics, the Missing Values number, 27444, is circled. ](media/image41.png "FlightDelaysWithAirportCodes dataset")

8.  Next, select the **CRSDepTime** column. Our model will approximate departure times to the nearest hour, but departure time is captured as an integer. For example, 8:37 am is captured as 837. Therefore, we will need to process the CRSDepTime column, and round it down to the nearest hour. To perform this rounding will require two steps, first you will need to divide the value by 100 (so that 837 becomes 8.37). Second, you will round this value down to the nearest hour (so that 8.37 becomes 8).

    ![In the FlightDelaysWithAirportCodes dataset, the CRSDepTime column is circled.. ](media/image42.png "FlightDelaysWithAirportCodes dataset ")

9.  Finally, we do not need all 20 columns present in the FlightDelaysWithAirportCodes dataset, so we will need to pare down the columns in the dataset to the 12.

10. Close the Visualize dialog, and go back to the design surface.

11. To perform our data munging, we have multiple options, but in this case, we've chosen to use an **Execute R Script** module, which will perform the following tasks:

    a.  Remove rows with missing values

    b.  Generate a new column, named "CRSDepHour," which contains the rounded down value from CRSDepTime

    c.  Pare down columns to only those needed for our model

12. To add the module, search for **Execute R Script** by entering "Execute R" into the Search experiment items box.

    ![Execute R is typed in the search field. Below, under R Language Modules, Execute R Script is selected.](media/image43.png "Search field")

13. Drag this module on to the design surface beneath your FlightDelaysWithAirportCodes dataset. Select the small circle at the bottom of the FlightDelaysWithAirportCodes dataset, drag and release when your mouse is over the circle found in the top left of the Execute R Script module. These circles are referred to as ports, and by taking this action you have connected the output port of the dataset with the input port of the Execute R Script module, meaning data from the dataset will flow along this path.

    ![On the Design Surface, an arrow points from FlightDelaysWithAirportCodes to Execute R Script.](media/image44.png "Design Surface")

14. In the **Properties** panel for **Execute R Script** module, select the **Double Windows** icon to maximize the script editor. 

    ![In the Properties panel, the Double Windows icon is selected.](media/image45.png "Properties panel")

15. Replace the script with the following (Press CTRL+A to select all then CTRL+V to paste)
    ```
    # Import data from the input port
    ds.flights <- maml.mapInputPort(1)

    # Delete rows containing missing values
    ds.flights <- na.omit(ds.flights)

    # Round departure times down to the nearest hour, and export the result as a new column named "CRSDepHour"
    ds.flights[, "CRSDepHour"] <- floor(ds.flights[, "CRSDepTime"] / 100) 

    # Trim the columns to only those we will use for the predictive model
    ds.flights = ds.flights[, c("OriginAirportCode","OriginLatitude", "OriginLongitude", "Month", "DayofMonth", "CRSDepHour", "DayOfWeek", "Carrier", "DestAirportCode", "DestLatitude", "DestLongitude", "DepDel15")]

    # Export the cleaned up data set
    maml.mapOutputPort("ds.flights");
    ```

16. Select the check mark in the bottom right to save the script (Do not worry if the formatting is off before selecting the check mark.)

    ![In the R Script Script Editor, the check mark at the bottom is selected.](media/image46.png "Script Editor")

17. Select **Save** on the command bar at the bottom to save your in-progress experiment. 

    ![Screenshot of the Save icon on the command bar.](media/image47.png "Save icon")

18. Select **Run** in the command bar at the bottom to run the experiment. 

    ![Screenshot of the Run icon on the command bar.](media/image48.png "Run icon")

19. When the experiment is finished running, you will see a finished message in the top right corner of the design surface, and green check marks over all modules that ran.

    ![On the Design Surface, an arrow points from FlightDelaysWithAirportCodes to Execute R Script, which has a green check mark circled.](media/image49.png "Design Surface")

20. You should run your experiment whenever you need to update the metadata describing what data is flowing through the modules, so that newly added modules can be aware of the shape of your data (most modules have dialogs that can suggest columns, but before they can make suggestions you need to have run your experiment).

21. To verify the results of our R script, right-click the left output port (Result Dataset) of the Execute R Script module and select **Visualize**.

    ![Visualize is selected from the Execute R Script Output port drop-down menu.](media/image50.png "Output port drop-down menu")

22. In the dialog that appears, scroll over to DepDel15 and select the column. In the statistics you should see that Missing Values reads 0. 

    ![In the Results Dataset for Execute R Script, under Statistics, Missing Values is now 0.](media/image51.png "Results Dataset ")

23. Now, select the CRSDepHour column, and verify that our new column contains the rounded hour values from our CRSDepTime column. 

    ![In the Results Dataset for Execute R Script, the CRSDepHour column is circled, and now displays times rounded to the nearest hour.](media/image52.png "Results Dataset ")

24. Finally, observe that we have reduced the number of columns from 20 to 12. Close the dialog. 

    ![In the Results Dataset for Execute R Script, the number of columns is now 12.](media/image53.png "Results Dataset ")

25. At this point the Flight Delay Data is prepared, and we turn to preparing the historical weather data.

### Task 5: Prepare the weather data

1.  To the right of the **FlightDelaysWithAirportCodes** dataset, add the **FlightWeatherWithAirportCodes** dataset. 

    ![On the Design Surface, FlightDelaysWithAirportCodes and FlightWeatherWithAirportCodes display side by side.](media/image54.png "Design Surface")

2.  Right-click the output port of the **FlightWeatherWithAirportCodes** dataset and select **Visualize**. 

    ![The Dataset for FlightWeatherWithAirportCode displays.](media/image55.png "Dataset")

3.  Observe that this data set has 406,516 rows and 29 columns. For this model, we are going to focus on predicting delays using WindSpeed (in MPH), SeaLevelPressure (in inches of Hg), and HourlyPrecip (in inches). We will focus on preparing the data for those features.

4.  In the dialog, select the **WindSpeed** column, and review the statistics. Observe that the Feature Type was inferred as String and that there are 32 Missing Values. Below that, examine the histogram to see that, even though the type was inferred as string, the values are all actually numbers (e.g. the x-axis values are 0, 6, 5, 7, 3, 8, 9, 10, 11, 13). We will need to ensure that we remove any missing values and convert WindSpeed to its proper type as a numeric feature. 

    ![In the Dataset for FlightWeatherWithAirportCode, under Statistics, Missing Values (32) and Feature Type (String Feature) are both circled. At the bottom of the WindSpeed stacked bar graph, the x-axis values are circled.](media/image56.png "Dataset")

5.  Next, select the **SeaLevelPressure** column. Observe that the Feature Type was inferred as String and there are 0 Missing Values. Scroll down to the histogram, and observe that many of the features are of a numeric value (e.g., 29.96, 30.01, etc.), but there are many features with the string value of M for Missing. We will need to replace this value of \"M\" with a suitable numeric value so that we can convert this feature to be a numeric feature. 

    ![In the Statistics section, Missing Values are now 0, Feature Type is still String Feature, and both are circled. At the bottom of the WindSpeed stacked bar graph, the x-axis values begin with M, which is circled.](media/image57.png "Statistics section")

6.  Finally, examine the **HourlyPrecip** feature. Observe that it too was inferred to have a Feature Type of String and is missing values for 374,503 rows. Looking at the histogram, observe that besides the numeric values, there is a value T (for Trace amount of rain). We need to replace T with a suitable numeric value and covert this to a numeric feature. 

    ![In the Statistics section, Missing Values are now 374503, Feature Type is still String Feature, and both are circled. At the bottom of the WindSpeed stacked bar graph, the x-axis values now begin with T, which is circled.](media/image58.png "Statistics section")

7.  To preform our data cleanup, we will use a Python script, in which we will perform the following tasks:

    a.  WindSpeed: Replace missing values with 0.0, and "M" values with 0.005

    b.  HourlyPrecip: Replace missing values with 0.0, and "T" values with 0.005

    c.  SeaLevelPressure: Replace "M" values with 29.92 (the average pressure)

    d.  Convert WindSpeed, HourlyPrecip, and SeaLevelPressure to numeric columns

    e.  Round "Time" column down to the nearest hour, and add value to a new column named "Hour"

    f.  Eliminate unneeded columns from the dataset

8.  Add an **Execute Python Script** by searching for Python.

    ![ML Studio is showning searching for Execute Python Script](media/image59.png "Azure Machine Learning Studio")

9.  Add the module below the **FlightWeatherWithAirportCode** module, and connect the output port of the **FlightWeatherWithAirportCode** module to the first input port of the Execute Python Script module. 

    ![On the Design Surface, FlightWeatherWithAirportCode has an arrow pointing to Execute Python Script, which has a green check mark.](media/image60.png "Design Surface")

10. In the **Properties** panel for the Execute Python Script:

    g.  Set the Python Version to **Anaconda 4.0/Python 3.5**

    h.  Select the **Double Windows** icon to open the script editor.

    ![In the Properties panel, the Python version is set to Anaconda 4.0/Python 3.5, and the double windows icon is circled.](media/image61.png "Properties panel")

11. Paste in the following script into the Python script window, and select the checkmark at the bottom right of the dialog (press CTRL+A to select all then CTRL+V to paste and then immediately select the checkmark \-- don\'t worry if the formatting is off before hitting the checkmark).
    ```
    # imports 
    import pandas as pd
    import math

    # The entry point function can contain up to two input arguments:
    #   Param<dataframe1>: a pandas.DataFrame
    #   Param<dataframe2>: a pandas.DataFrame
    def azureml_main(dataframe1 = None, dataframe2 = None):
        
        # Round weather Time down to the next hour, since that is the hour for which we want to use flight dataframe1
        # Add the rounded Time to a new column named "Hour," and append that column to the dataframe1
        dataframe1["Hour"] = dataframe1["Time"].apply(roundDown)
        
        # Replace any missing HourlyPrecip and WindSpeed values with 0.0
        dataframe1["HourlyPrecip"] = dataframe1["HourlyPrecip"].fillna('0.0')
        dataframe1["WindSpeed"] = dataframe1["WindSpeed"].fillna('0.0')
        
        # Replace any WindSpeed values of "M" with 0.005
        dataframe1["WindSpeed"] = dataframe1['WindSpeed'].replace(['M'], '0.005')
        
        # Replace any SeaLevelPressure values of "M" with 29.92 (the average pressure)
        dataframe1["SeaLevelPressure"] = dataframe1['SeaLevelPressure'].replace(['M'], '29.92')
        
        # Replace any HourlyPrecip values of "T" (trace) with 0.005
        dataframe1["HourlyPrecip"] = dataframe1['HourlyPrecip'].replace(['T'], '0.005')
        
        # Convert our WindSpeed, SeaLevelPressure, and HourlyPrecip columns to numeric
        dataframe1[['WindSpeed','SeaLevelPressure', 'HourlyPrecip']] = dataframe1[['WindSpeed','SeaLevelPressure', 'HourlyPrecip']].apply(pd.to_numeric)

        # Pare down the variables in the Weather dataset to just the columns being used by the model
        df_result = dataframe1[['AirportCode', 'Month', 'Day', 'Hour', 'WindSpeed', 'SeaLevelPressure', 'HourlyPrecip']]
        
        # Return value must be of a sequence of pandas.DataFrame
        return df_result

    def roundDown(x):
        z = int(math.floor(x/100.0))
        return z 
    ```

12. Run the experiment. Currently it should appear as follows: 

    ![In the Design Surface, on the left, FlightDelaysWithAirportCodes has an arrow pointing down to Execute R Script. On the right, FlightWeatherWithAirportCodes has an arrow pointing down to Execute Python Script.](media/image62.png "Design Surface")

Note: If you receive an error in the Python script that .to\_numeric does not exist, go back and verify that you selected the proper Python version.

13. Right-click the first output port of the Execute Python Script module, and select Visualize. 

    ![In the Results dataset for Execute Python Script, the columns value, which is 7, is circled.](media/image63.png "Results dataset")

14. In the statistics, verify that there are now only the 7 columns we are interested in, and that WindSpeed, SeaLevelPressure, and HourlyPrecip are now all Numeric Feature types and that they have no missing values. 

    ![The Missing Values (0) and Feature Type (Numeric Feature) line items display.](media/image64.png "Statistics section")

### Task 6: Join the Flight and Weather datasets

1.  With both datasets ready, we want to join them together so that we can associate historical flight delays with the weather data at departure time.

2.  Drag a **Join Data** module onto the design surface, beneath and centered between both Execute R and Python Script modules. Connect the output port (1) of the Execute R Script module to input port (1) of the Join Data module, and the output port (1) of the Execute Python Script module to the input port (2) of the Join Data module. 

    ![On the Design Surface, Execute R Script is on the left, and Execute Python Script is on the right. Both have arrows pointing down to Join Data.](media/image65.png "Design Surface")

3.  In the **Properties** panel for the Join Data module, relate the rows of data between the two sets L (the flight delays) and R (the weather). Select **Launch Column selector** under **Join key columns for L.**

    ![The Join Data Propties dialog from ML Studio is shown. The Launch column selector is being selected.](media/image66.png "Join Data Properties dialog box")

4.  Set the Join key columns for L to include **OriginAirportCode, Month, DayofMonth,** and **CRSDepHour**, and select the check box in the bottom right. 

    ![On the Properties Panel, under Select columns, By name is selected. Under Selected columns, AirportCode, Month, Day, and Hour are circled. At the bottom, the check box is circled.](media/image67.png "Properties Panel")

5.  Select **Launch Column selector** under **Join key columns for R**. Set the join key columns for R to include **AirportCode,** **Month, Day,** and **Hour,** and select the check box in the bottom right. 

    ![On the Properties Panel, under Select columns,in the left pane, By name is selected. In the right pane, under Selected columns, OriginAirportCode, Month, DayofMonth, and CRSDepHour are circled. At the bottom, the check box is circled.](media/image68.png "Properties panel")

6.  Leave the Join Type at Inner Join, and uncheck **Keep right key columns in joined table** (so that we do not include the redundant values of AirportCode, Month, Day, and Hour). 

    ![The Properties panel displays with the previously mentioned settings.](media/image69.png "Properties panel")

7.  Next, drag an **Edit Metadata** module onto the design surface below the Join Data module, and connect its input port to the output port of the Join Data module. We will use this module to convert the fields that were unbounded String feature types, to the enumeration like Categorical feature.

    ![On the Design Surface, Join Data points down to Edit Metadata.](media/image70.png "Design Surface")

8.  On the **Properties** panel of the Edit Metadata module, select **Launch column selector** and set the Selected columns to **DayOfWeek, Carrier, DestAirportCode, and OriginAirportCode**, and select the checkbox in the bottom right. 

    ![On the Properties Panel, under Select columns, in the left pane, With Rules is selected. In the right pane, all fields display with the previously mentioned settings. At the bottom, the check box is circled.](media/image71.png "Properties Panel")

9.  Set the Categorical drop down to **Make categorical**. 

    ![The Properties Panel displays with the previously mentioned settings, and the Categorical field, which is set to Make categorical, is circled.](media/image72.png "Properties Panel")

10. Drag a **Select Columns in Dataset** module onto the design surface, below the Edit Metadata module. Connect the output of the Edit Metadata module to the input of the Select Columns in Dataset module. 

    ![On the Design surface, Edit Metadata points down to Select Columns in Dataset.](media/image73.png "Design surface")

11. Launch the column selector, and choose **Begin With** **All Columns**, choose **Exclude** and set the selected columns to exclude: **OriginLatitude, OriginLongitude, DestLatitude**, and **DestLongitude**. 

    ![The Select columns section, in the left pane, With Rules is selected. In the right pane, fields displays with the previously defined settings.](media/image74.png "Select columns section")

12. Save your experiment.

13. **Run** the experiment, to verify everything works as expected, and when completed select Visualize by right-clicking on the output of the **Select Columns in Dataset** module. You will see the joined datasets as output.

    ![The Results dataset for Select Columns in Dataset display.](media/image75.png "Results dataset")

14. The model should now look like the following. 

    ![This section of the Design surface begins on the left with FlightDelaysWithAirportCodes, which has an arrow pointing down to Execute R Script, which has an arrow pointing down to Join Data. On the right, FlightWeatherWithAirportCodes points down to Execute Python Script, which joins Execute R Script in pointing down to Join Data. Join Data points down to Edit Metadata, which points down to Select Columns in Dataset.](media/image76.png "Design Surface")

### Task 7: Train the model

AdventureWorks Travel wants to build a model to predict if a departing flight will have a 15-minute or greater delay. In the historical data they have provided, the indicator for such a delay is found within the DepDel15 (where a value of 1 means delay, 0 means no delay). To create a model that predicts such a binary outcome, we can choose from the various Two-Class modules that Azure ML offers. For our purposes, we begin with a Two-Class Logistic Regression. This type of classification module needs to be first trained on sample data that includes the features important to making a prediction and must also include the actual historical outcome for those features.

The typical pattern is to split the historical data so a portion is shown to the model for training purposes, and another portion is reserved to test just how well the trained model performs against examples it has not seen before.

1.  To create our training and validation datasets, drag a **Split Data** module beneath Select Columns in Dataset, and connect the output of the Select Columns in Dataset module to the input of the Split Data module. 

    ![On the Design Surface, Select Columns in Dataset has an arrow pointing down to Split Data.](media/image77.png "Design Surface")

2.  On the **Properties** panel for the Split Data module, set the Fraction of rows in the first output dataset to **0.7** (so 70% of the historical data will flow to output port 1). Set the Random seed to **7634**. 

    ![On the Properties Panel for the Split Data module, the Fraction of rows in the first output dataset field is set to 0, and is circled. The Random seed field is set to 7634, and is also circled.](media/image78.png "Properties Panel")

3.  Next, add a Train Model module and connect it to output 1 of the Split Data module.

    ![On the Design Surface, Split Data has an arrow pointing down to Train Model.](media/image79.png "Design Surface")

4.  On the **Properties** panel for the Train Model module, set the Selected columns to **DepDel15**.

    ![Under Train Model, under Label column, under Selected columns, Column names is DepDel15.](media/image80.png "Train Model section")

5.  Drag a Two-Class Logistic Regression module above and to the left of the Train Model module and connect the output to the leftmost input of the Train Model module
    
    ![On the Design Surface, on the left, Two-Class Logistics Regression and on the right, Split Data both have arrows pointing down to Train Model.](media/image81.png "Design Surface")

6.  Below the Train Model drop a Score Model module. Connect the output of the Train Model module to the leftmost input port of the Score Model and connect the rightmost output of the Split Data module to the rightmost input of the Score Model.

    ![In addition to the previously described Design Surface, both Train Model and Split Data both have arrows pointing down to Score Model.](media/image82.png "Design Surface")

7.  Save the experiment.

8.  Run the experiment.

9.  When the experiment is finished running (which takes a few minutes), right-click on the output port of the Score Model module and select **Visualize** to see the results of its predictions. **You should have a total of 13 columns**.

    ![In the Scored Dataset for Score Model, the number of columns is 13.](media/image83.png "Scored dataset")

10. If you scroll to the right so that you can see the last two columns, observe there are Scored Labels and Scored Probabilities columns. The former is the prediction (1 for predicting delay, 0 for predicting no delay) and the latter is the probability of the prediction. In the following screenshot, **for example**, the last row shows a delay predication with a 53.1% probability.

    ![Scored Labels and Scored Probabilities columns display.](media/image84.png "Scored Labels and Scored Probabilities columns")

11. While this view enables you to see the prediction results for the first 100 rows, if you want to get more detailed statistics across the prediction results to evaluate your model\'s performance, you can use the Evaluate Model module.

12. Drag an Evaluate Model module on to the design surface beneath the Score Model module. Connect the output of the Score Model module to the leftmost input of the Evaluate Model module.

    ![On the Design Surface, Score Model has an arrow pointing down to Evaluate Model.](media/image85.png "Design Surface")

13. Run the experiment.

14. When the experiment is finished running, right-click the output of the Evaluate Model module and select **Visualize**. In this dialog box, you are presented with various ways to understand how your model is performing in the aggregate. While we will not cover how to interpret these results in detail, we can examine the ROC chart that tells us that at least our model (the blue curve) is performing better than random (the light gray straight line going from 0,0 to 1,1)---which is a good start for our first model!

    ![On the Evaluation results line graph the x-axis is labeled False Positive Rate, and the y-axis is labeled True Positive Rate. The line arches upward from lower left to top right.](media/image86.png "Evaluation results line graph")

    ![The Evaluation results data lists data for the following categories: True Positive, False Negative, Accuracy, Precision, False Positive, True Negative, Recall, F1 Score, Threshold, and AUC.](media/image87.png "Evaluation results data")

### Task 8: Operationalize the experiment

1.  Now that we have a functioning model, let us package it up into a predictive experiment that can be called as a web service.

2.  In the command bar at the bottom, select **Set Up Web Service** and then select **Predictive Web Service \[Recommended\]**. (If Predictive Web Service is grayed out, run the experiment again) 
    
    ![On the Command bar, Set Up Web Service is selected. From it\'s sub-menu, Predictive Web Service \[Recommended\] is selected.](media/image88.png "Command bar")

3.  A copy of your training experiment is created, and a new tab labeled Predictive Experiment is added, which contains the trained model wrapped between web service input (e.g. the web service action you invoke with parameters) and web service output modules (e.g., how the result of scoring the parameters are returned). 

    ![The Design Surface for Big Data Hands-on Lab \[Predicitive Exp.\] now displays from the beginning Web service input through Web service output.](media/image89.png "Design Surface")

4.  We will make some adjustments to the web service input and output modules to control the parameters we require and the results we return.

5.  Move the Web Service Input module down, so it is to the right of the Join Data module. Connect the output of the Web service input module to input of the Edit Metadata module.
    
    ![On this Design Surface, Web service input has an arrow pointing down to Edit Metadata.](media/image90.png "Design Surface")

6.  Right-click the line connecting the Join Data module and the Edit Metadata module and select **Delete**.

    ![On this Design Surface, Web service input has an arrow pointing down to Edit Metadata. This time, however, Edit Metadata has a warning icon, a red circle with an exclamation point in it.](media/image91.png "Design Surface")

7.  In between the **Join Data** and the **Edit Metadata** modules, drop a **Select Columns in Dataset module**. Connect the Join Data module's output to the Select Columns module's input, and the Select Columns output to the Edit Metadata module's input.

    ![The Select Columns in Dataset module has been added to the map in between the Join Data and the Edit Metadata.](media/image92.png "Design surface")

8.  Click the **Select Columns in Dataset module**, and in the **Properties** panel click **Launch column select. In the Select column** dialog select **All Columns** for Begin With and then select **Exclude**. Enter columns **DepDel15, OriginLatitude, OriginLongitude, DestLatitude,** and **DestLongitude**. 

    ![The Properties Panel for Select columns fields display the previously mentioned settings.](media/image93.png "Properties Panel")

9.  This configuration will update the web service metadata so that these columns do not appear as required input parameters for the web service.

    ![In the Select Columns in Dataset Section, under Select columns\\All columns, DepDel15, OriginLatitude, OriginLongitude, DestLatitude, and DestLongitude are listed under column names to be excluded.](media/image94.png "Select Columns in Dataset Section")

10. Select the **Select Columns in Dataset** module that comes after the **Edit Metadata** module, and delete it.

11. Connect the output of the **Edit Metadata** module directly to the right input of the Score Model module.

    ![On this Design surface, Select Columns in Dataset on the left, and Web service input on the right both have arrows pointing to Edit Metadata, which in turn has an arrow pointing to Score Model. AdventureWorks Travel (trai\...) also has an arrow pointing to Score Model](media/image95.png "Design surface")

12. As we removed the latitude and longitude columns from the dataset to remove them as input to the web service, we have to add them back in before we return the result so that the results can be easily visualized on a map.

13. To add these fields back, begin by deleting the line between the **Score Model** and **Web service output**.

14. Drag the **AirportCodeLocationLookupClean** dataset on to the design surface, positioning it below and to the right of the Score Model module.

    ![On this Design Surface, AirportCodeLocationLookup is listed under Score Model, although no arrow joins them.](media/image96.png "Design Surface")

15. Add a **Join Data** module, and position it below and to the left of the **AirportCodeLocationLookupClean** module.

    ![The Join Data module is shown below and to the left of the AirportCodeLocationLookupClean module which is below and to the right of the Score Model module.](media/image97.png "Design surface")

16. Connect the output of the **Score Model** module to the leftmost input of the **Join Data** module and the output of the **AirportCodeLocationLookupClean** to the rightmost input of the **AirportCodeLocationLookupClean** module.

    ![On this Design Surface, both Score Model and AirportCodeLocationLookup have arrows pointing down to Join Data.](media/image98.png "Design Surface")

17. Click the **Join Data** module, and in the **Properties** panel for the **Join key columns for L** launch the column selector and then under Begin With choose **No Columns** and set the Include column names to **OriginAirportCode,** then click the Check to confirm. For the **Join key columns for R**, launch the column selector move **AIRPORT** over to the Selected Column side and click the Check to confirm. Uncheck **Keep right key columns** in joined table.

    ![Fields in the Join Data section are set to the previously defined settings.](media/image99.png "Join Data section")

18. Add **a Select Columns in Dataset** module beneath the **Join Data** module. Connect the **Join Data** output to the input of the **Select Columns in Dataset** module.

    ![The new Select Columns in Dataset module is shown believe the Join Data module. The output of the Join Data is connected to the input of the Select Columns in Dataset.](media/image100.png "Design surface")

19. Click the **Select Columns in Dataset** module, and in the **Property** panel, under Begin With select **All Columns,** and set the Selected columns to **Exclude** the columns: **AIRPORT\_ID** and **DISPLAY\_AIRPORT\_NAME** and click the Check to confirm.

    ![Under Project Columns, under select columns\\Selected columns\\All columns, both the columns AIRPORT\_ID and DISPLAY\_AIRPORT\_NAME are set to be excluded: ](media/image101.png "Project Columns section")

20. Add an **Edit Metadata** module below the **Select Columns in Dataset**. Connect the output of the **Select Columns in Dataset** module to the input of the **Edit Metadata** module.

    ![The Select Columns in Dataset module is shown connected to the new Edit Metadata module. The output of the Join Data is connected to the input of the Select Columns in Dataset. The output of the Select Columns in Dataset is connected to the input of the Edit Metadata module.](media/image102.png "Design surface")

21. Click the **Edit Metadata** and in the **Properties** under Begin With, click No Columns and then include the column names **LATITUDE** and **LONGITUDE** and then click the Checkmark to confirm. Back in the properites for the Edit Metadata, in the New column names enter: **OriginLatitude**, **OriginLongitude**.

    ![In the Metadata Editor, fields are set to the previously defined settings.](media/image103.png "Metadata Editor")

22. Connect the output of the Edit Metadata to the input of the web service output module.

    ![On this Design Surface, Edit Metadata points down to Web service output.](media/image104.png "Design Surface")

23. Run the experiment.

    ![The experience map is shown after the new Predictive expertient run.](media/image105.png "Design surface")

24. When the experiment is finished running, select **Deploy Web Service**, **Deploy Web Service \[NEW\] Preview**. 

    ![On the Menu bar, Deploy Web Service is selected, and from it\'s sub-menu, Deploy Web Service \[New\] Preview is selected.](media/image106.png "Menu bar")

25. On the Deploy experiment as a web service page in the **Price Plan** drop down select **Create New...**. Then select **Dev Test** as the Plan Name. Finally select **Standard DevTest (FREE)** under **Monthly Plan** **Options**.

    ![The Deploy as a Web Service page is shown with the Create New price plan selected, the Plan Name is Dev Test and the Standard DevTest Monthly plan has been chosen.](media/image107.png "Deploy experiment as a web service page")

26. Scroll down and click **Deploy**.

    ![The deploy button is shown being clicked after the web service was setup.](media/image108.png "Deploy button")

27. When the deployment is complete, you will be taken to the Web Service Quickstart page. Select the **Consume** tab. !

    [On the Web Service Quickstart page, the Consume tab is selected.](media/image109.png "Web Service Quickstart page")

28. Leave the Consume page open for reference during [Exercise 4, Task 1](#task-1-create-azure-ml-linked-service). At that point, you need to copy the Primary Key and Batch Requests Uri (omitting the querystring -- "?api-version=2.0") to notepad for use later.

    ![The Primary Key and Batch Request Keys are shown being copied to a text file for use later in this Hands-on Lab.](media/image110.png "Basic consumption info")

    ![The Primary Key and Batch Request Keys are shown copied to a text file for use later in this Hands-on Lab.](media/image111.png "Primary key")

## Exercise 2: Setup Azure Data Factory

Duration: 20 minutes

In this exercise, attendees will create a baseline environment for Azure Data Factory development for further operationalization of data movement and processing. You will create a Data Factory service, and then install the Data Management Gateway which is the agent that facilitates data movement from on-premises to Microsoft Azure.

### Task 1: Connect to the Lab VM

1.  **Note**: If you are already, connected to your Lab VM, skip to Task 2.

2.  From the left side menu in the Azure portal, click on **Resource groups**, then enter your resource group name into the filter box, and select it from the list. 

    ![On the Azure Portal, in the left menu, Resource groups is selected. In the Resource groups blade, bigdata is typed in the Subscriptions search field. In the results, bigdatakyle is selected.](media/image14.png "Azure Portal")

3.  Next, select your lab virtual machine from the list. 

    ![the kylelab virtual machine is selected from Lab virtual machine list.](media/image15.png "Lab virtual machine list")

4.  On your Lab VM blade, select **Connect** from the top menu. 

    ![On the top menu of the Lab VM blade, the Connect button is selected.](media/image16.png "Lab VM blade")

5.  Download and open the RDP file.

6.  Select Connect, and enter the following credentials:

    -   User name: demouser

    -   Password: Password.1!!

### Task 2: Download and stage data to be processed

1.  Once you have logged into the Lab VM, open a web browser.

2.  Download the AdventureWorks sample data from <http://bit.ly/2zi4Sqa>.

3.  Extract it to a new folder called **C:\\Data**.

### Task 3: Install and configure Azure Data Factory Integration Runtime on the Lab VM

1.  To download the latest version of Azure Data Factory Integration Runtime, go to <https://www.microsoft.com/en-us/download/details.aspx?id=39717>

    ![The Azure Data Factory Integration Runtime Download webpage displays.](media/image112.png "Azure Data Factory Integration Runtime Download webpage")

2.  Select Download, then choose the download you want from the next screen. 

    ![Under Choose the download you want, IntegrationRuntime\_3.0.6464.2 (64-bit).msi is selected.](media/image113.png "Choose the download you want section")

3.  Run the installer, once downloaded.

4.  When you see the following screen, select Next. 

    ![The Welcome page in the Microsoft Integration Runtime Setup Wizard displays.](media/image114.png "Microsoft Integration Runtime Setup Wizard")

5.  Check the box to accept the terms and select Next. 

    ![On the End-User License Agreement page, the check box to accept the license agreement is selected, as is the Next button.](media/image115.png "End-User License Agreement page")

6.  Accept the default Destination Folder, and select Next. 

    ![On the Destination folder page, the destination folder is set to C;\\Program Files\\Microsoft Integration Runtime\\ and the Next button is selected.](media/image116.png "Destination folder page")

7.  Select Install to complete the installation. 

    ![On the Ready to install Microsoft Integration Runtime page, the Install button is selected.](media/image117.png "Ready to install page")

8.  Select Finish once the installation has completed. 

    ![On the Completed the Microsoft Integration Runtime Setup Wizard page, the Finish button is selected.](media/image118.png "Completed the Wizard page")

9.  After clicking Finish, the following screen will appear. Keep it open for now. You will come back to this screen once the Data Factory in Azure has been provisioned, and obtain the gateway key so we can connect Data Factory to this "on-premises" server.

    ![The Microsoft Integration Runtime Configuration Manager, Register Integration Runtime page displays.](media/image119.png "Register Integration Runtime page")

### Task 4: Create an Azure Data Factory

1.  Launch a new browser window, and navigate to the Azure portal (<https://portal.azure.com>). Once prompted, log in with your Microsoft Azure credentials. If prompted, choose whether your account is an organization account or a Microsoft account. This will be based on which account was used to provision your Azure subscription that is being used for this lab.

2.  From the top left corner of the Azure portal, select **+ Create a resource**, and select **Data + Analytics**, then select **Data Factory**.

    ![The Azure Portal is shown creating a Data Factory.](media/image120.png "Azure Portal")

3.  On the New data factory blade, enter the following:

    a.  Name: **Provide a new unique name, alpha-numeric 3-24 characters lowercase (this is a dns name)**

    b.  Subscription: **Select your subscription**

    c.  Resource Group: **Choose Use existing, and select the Resource Group you created when deploying the lab prerequisites**

    d.  Version: **Select V1**

    e.  Location: **Select one of the available locations from the list nearest the one used by your Resource Group**

    f.  Select **Create**

    ![The New Data Factory blade fields are set to the previously defined settings.](media/image121.png "New Data Factory blade")

4.  Once the deployment is completed, you will receive a notification that it succeeded. 

    ![Under Notifications, a Deployment succeeded message displays.](media/image122.png "Deployment succeeded message")

5.  Select the **Go to resource button**, to navigate to the newly created Data Factory.

6.  On the Data Factory blade, select **Author and Deploy** under Actions. 

    ![In the bigdata-adf blade, under Actions, the Author and deploy option is selected.](media/image123.png "bigdata-adf blade")

7.  Next, select **...More**, then New integration runtime (gateway).
    
    ![To the right of New data store, the More ellipses is selected. From its sub-menu, New integration runtime (gateway) is selected.](media/image124.png "New integration runtime option")

8.  Enter an **Integration runtime name**, such as bigdatagateway-\[initials\], and select **OK**. 

    ![In the New integration runtime (gateway) blade, Create is selected. In the Create blade, the Integration runtime name field is set to bigdatagateway-kyle.](media/image125.png "New integration runtime (gateway), and Create blades")

9.  On the Configure screen, copy the key1 value by selecting the Copy button, then select OK. 

    ![In the New integration runtime (gateway) blade, Configure is selected. In the Configure blade, under Authentication Key, the key1 key and its copy button are circled.](media/image126.png "New integration runtime (gateway), and Configure blades")

10. *Don't close the current screen or browser session.*

11. Go back to the Remote Deskop session of the **Lab VM.**

12. Paste the **key1** value into the box in the middle of the Microsoft Integration Runtime Configuration Manager screen.

    ![The Microsoft Integration Runtime Configuration Manager Register Integration Runtime page displays.](media/image127.png "Microsoft Integration Runtime Configuration Manager")

13. Select **Register**.

14. It will take a minute or two to register. If it takes more than a couple of minutes, and the screen does not respond or returns an error message, close the screen by clicking the **Cancel** button.

15. The next screen will be New Integration Runtime (Self-hosted) Node. Select Finish. 

    ![The Microsoft Integration Runtime Configuration Manager New Integration Runtime (Self-hosted) Node page displays.](media/image128.png "Microsoft Integration Runtime Configuration Manager")

16. You will then get a screen with a confirmation message. 

    ![The Microsoft Integration Runtime Configuration Manager Register Integration Runtime (Self-hosted) page displays.](media/image129.png "Microsoft Integration Runtime Configuration Manager")

17. Select the **Launch Configuration Manager** button to view the connection details. 

    ![The Microsoft Integration Runtime Configuration Manager Node is connected to the cloud service page displays with connection details.](media/image130.png "Microsoft Integration Runtime Configuration Manager")

18. You can now return to the Azure portal, and click **OK** twice to complete the Integration Runtime setup.

19. You can view the Integration Runtime by expanding Integration runtimes on the Author and Deploy blade. 

    ![In the bigdata-adf blade, under New data store, Integration runtimes (Gateways) is expanded, and bigdatagateway-kyle displays below. Both are circled.](media/image131.png "bigdata-adf blade")

20. Close the Author and Deploy blade, to return to the the Azure Data Factory blade. Leave this open for the next exercise.

## Exercise 3: Develop a data factory pipeline for data movement

Duration: 20 minutes

In this exercise, you will create an Azure Data Factory pipeline to copy data (.CSV file) from an on-premises server (Lab VM) to Azure Blob Storage. The goal of the exercise is to demonstrate data movement from an on-premises location to Azure Storage (via the Integration Runtime). You will see how assets are created, deployed, executed, and monitored.

### Task 1: Create copy pipeline using the Copy Data Wizard

1.  On your Azure Data Factory blade in the Azure portal, select **Copy Data (PREVIEW)**, under Actions. 
    
    ![On the Azure Data Factory blade, under Actions, the Copy data (PREVIEW) option is selected.](media/image132.png "Azure Data Factory blade")

2.  This will launch a new browser window. Log in with the same credentials you used to create the Data Factory.

3.  In the new browser window, enter the following:

    -   Task name: **CopyOnPrem2AzurePipeline**

    -   Task description: (Optional) **"This pipeline copies timesliced CSV files from on-premises virtual machine C:\\Data to Azure Blob Storage as a continuous job."**

    -   Task cadence (or) Task schedule: **Select Run regularly on schedule**.

    -   Recurring pattern: **Select Monthly, and every 1 month.**

    -   Start date time (UTC): **03/01/2017 12:00 am**

    -   End date time (UTC): **12/31/2099 11:59 pm**

    ![Set the ADF pipeline copy activity properties by setting the Task Name to CopyOnPrem2AzurePipeline, adding a description, setting the Task cadence to Run regularly on a Monthly schedule, every 1 month.](media/image133.png "Properties dialog box")

4.  Select **Next.**

5.  On the Source screen, select **File System**, then select **Next**.

    ![The source data store screen is shown and the File system option has been selected.](media/image134.png "Source data store")

6.  From the Specify File server share connection screen, enter the following:

    -   Connection name: **OnPremServer**

    -   Integration Runtime/Gateway: **Select the Integration runtime created previously in this exercise (this value should already be populated)**

    -   Path: **C:\\Data**

    -   Credential encryption: **By web browser**

    -   User name: Enter **demouser**

    -   Password: Enter **Password.1!!**

7.  Select **Next** 
    
    ![On the Copy Data (bigdatalab-adf) Specify File server share connection page, fields are set to the previously defined values.](media/image135.png "Copy Data (bigdatalab-adf) Specify File page")

8.  On the **Choose the input file or folder** screen, select the folder **FlightsAndWeather**, and select **Choose**. 

    ![In the Choose the input file or folder section, the FlightsandWeather folder is selected.](media/image136.png "Choose the input file or folder page")

9.  On the next screen, check the **Copy files recursively** check box, and select **Next**. 

    ![On the Choose the input file or folder page, the check box for Copy files recursively is selected.](media/image137.png "Choose the input file or folder page")

10. On the File format settings page, leave the default settings, and select **Next**. 

    ![The Copy Data (bigdatalab-adf) File format settings page displays.](media/image138.png " Copy Data (bigdatalab-adf) File format settings page")

11. On the Destination screen, select **Azure Blob Storage**, and select **Next**. 
    
    ![In the left pane of the Copy Data (bigdatalab-adf) Destination data store page, Destination is selected. On the Connect to a data store tab, Azure Blob Storage is selected.](media/image139.png "Copy Data (bigdatalab-adf) Destination data store page")

12. On the Specify the Azure Blob storage account screen, enter the following and then click **Next.**

    -   Connection name: **BlobStorageOutput**

    -   Account selection method: **From Azure subscriptions**

    -   Azure Subscription: **Select your subscription**

    -   Storage account name: Select **\<YOUR\_APP\_NAME\>sparkstorage**. *Make sure you select the storage account with the **sparkstorage** suffix, or you will have issues with subsequent exercises.* This ensures data will be copied to the storage account that the Spark cluster users for its data files. ![On the Copy Data (bigdatalab-adf) Specify the Azure Blob storage account page, fields are set to the previously defined settings.](media/image140.png "Copy Data (bigdatalab-adf) Specify the Azure Blob storage account page")

13. From the **Choose the output file or folder** tab, enter the following:

    -   Folder path: **sparkcontainer/FlightsAndWeather/{Year}/{Month}/**

    -   Filename: **FlightsAndWeather.csv**

    -   Year: Select **yyyy** from the drop down

    -   Month: Select **MM** from the drop down

    -   Select **Next**.

        ![On the Copy Data (bigdatalab-adf) Choose the output file or folder page, fields are set to the previously defined settings.](media/image141.png "Copy Data (bigdatalab-adf) Choose the output file or folder page")

14. On the File format settings screen, check the **Add header to file** checkbox, then select **Next**. 

    ![On the Copy Data (bigdatalab-adf) File format settings page, the check box for Add header to file is selected.](media/image142.png "Copy Data (bigdatalab-adf) File format settings page")

15. On the **Settings** screen, select **Skip all incompatible rows** under Actions, then select **Next**. 
    
    ![On the Copy Data (bigdatalab-adf) page, in the left pane, Settings is selected. In the right pane, under Settings, the Actions field is set to Skip all incompatible rows (copy succeeds), and is selected.](media/image143.png "Copy Data (bigdatalab-adf) Settings page")

16. Review settings on the **Summary** tab, but **DO NOT click Next**. 

    ![The Copy Data (bigdatalab-adf) page, in the left pane, Summary is selected. In the right pane, Summary details display.](media/image144.png "Copy Data (bigdatalab-adf) Summary page")

17. Scroll down on the summary page until you see the **Copy Settings** section. Select **Edit** next to **Copy Settings**. 

    ![In the Copy settings section, the Edit button is selected.](media/image145.png "Copy settings section")

18. Change the following Copy settings

    -   Concurrency: Set to **10**

    -   Execution priority order: Change to **OldestFirst**

    -   Select **Save** 

        ![In the Copy settings section, the Concurrency and Execution priority order fields are set to the previously defined settings, and are circled. At the top, the Save button is selected.](media/image146.png "Copy settings section")

19. After saving the Copy settings, select **Next** on the Summary tab.

20. On the **Deployment** screen you will see a message that the deployment in is progress, and after a minute or two that the deployment completed. 

    ![On the Copy Data (bigdatalab-adf) page, in the left pane, Deployment is selected. In the right, Deployment complete pane, a list of tasks that passed displays. At the bottom, the live link to Click here to monitor copy pipeline displays.](media/image147.png "Copy Data (bigdatalab-adf) Deployment page")

21. Select the **Click here to monitor copy pipeline** link at the bottom of the **Deployment** screen.

22. Adjust the Start time in the window, as follows, and then select **Apply**. 
    
    ![In the bigdatalab-adf / CopyOnPrem2AzurePipeline window, the Start time has been changed to 03/01/2017 12:00 am, and the Apply button is selected.](media/image148.png "bigdatalab-adf / CopyOnPrem2AzurePipeline window")

23. From the **Data Factory Resource Explorer**, you should see the pipeline activity status running and then turn to **Ready**. This indicates the CSV files are successfully copied from your VM to your Azure Blob Storage location. 
    
    ![In the Data Factory Resource Explorer window, the pipeline activity status displays as Ready.](media/image149.png "Data Factory Resource Explorer")

## Exercise 4: Operationalize ML scoring with Azure ML and Data Factory

Duration: 20 minutes

In this exercise, you will extend the Data Factory to operationalize the scoring of data using the previously created Azure Machine Learning (ML) model.

### Task 1: Create Azure ML Linked Service

1.  Return to the Azure Data Factory blade in the Azure portal

2.  Select **Author and Deploy** below **Actions**. 
    
    ![In the Data Factory blade, under Actions, Author and deploy is selected.](media/image150.png "Data Factory blade")

3.  On the Author and Deploy blade, select **...More**, the select **New Compute**. 

    ![In the Author and Deploy blade, the More ellipses is selected, and from its sub-menu, New compute is eslected.](media/image151.png "Author and Deploy blade")

4.  Select **Azure ML** from the New Compute list.

    ![Azure ML is selected from the New compute list.](media/image152.png "New compute list")

5.  In the new window, update the JSON using the guidance below. Earlier in the lab you captured information on the ML Web Service that will be used here.

    a.  The value of **mlEndpoint** below is your web service's **Batch Request URL,** remember to ***remove*** the query string (e.g., "?api\_version=2.0").

    b.  **apiKey** is the **Primary Key** of your web service.

    c.  Delete the following three settings (updateResourceEndpoint, servicePrincipalId, servicePrincipalKey).

    d.  Your tenant string should be populated automatically.

    The JSON will resemble the following:
    ```
    {
        "name": "AzureMLLinkedService",
        "properties": {
            "type": "AzureML",
            "description": "",
            "typeProperties": {
                "mlEndpoint": "<Specify the batch scoring URL>",
                "apiKey": "<Specify the published workspace model???s API key>",
                "tenant": "<Specify your tenant string>"
            }
        }
    }
    ```

6.  Select **Deploy**.

    ![Screenshot of the Deploy button.](media/image153.png "Deploy button")

### Task 2: Create Azure ML input dataset

1.  Still on the Author and Deploy blade, select **...More**.

2.  To create a new dataset that will be copied into Azure Blob storage, select **New dataset** from the top. 
    
    ![On the Author and Deploy blade, the More ellipses is selected, and from its drop-down menu, New dataset is selected.](media/image154.png "Author and Deploy blade")

3.  Select **Azure Blob storage** from the list of available datasets.

    ![Azure Blob storage is selected from the New dataset list.](media/image155.png "New dataset list")

4.  Replace the JSON text in the draft window with following JSON.
    ```
    {
        "name": "PartitionedBlobInput",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "BlobStorageOutput",
            "typeProperties": {
                "fileName": "FlightsAndWeather.csv",
                "folderPath": "sparkcontainer/FlightsAndWeather/{Year}/{Month}/",
                "format": {
                    "type": "TextFormat"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }
    ```
    
5.  Select **Deploy**.

    ![Screenshot of the Deploy button.](media/image153.png "Deploy button")

### Task 3: Create Azure ML scored dataset

1.  Select **...More** again, and select **New dataset**.

    ![On the Author and Deploy blade, the More ellipses is selected, and from its drop-down menu, New dataset is selected.](media/image154.png "Author and Deploy blade")

2.  Select **Azure Blob storage** from the list of available datasets.

    ![Azure Blob storage is selected from the New dataset list.](media/image155.png "New dataset list")

3.  Replace the JSON text in the draft window with following JSON.
    ```
    {
        "name": "ScoredBlobOutput",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "BlobStorageOutput",
            "typeProperties": {
                "fileName": "Scored_FlightsAndWeather{Year}{Month}.csv",
                "folderPath": "sparkcontainer/ScoredFlightsAndWeather",
                "format": {
                    "type": "TextFormat"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            }
        }
    }
    ```
4.  Select **Deploy**.

    ![Screenshot of the Deploy button](media/image153.png "Deploy button")

### Task 4: Create Azure ML predictive pipeline

1.  Select **...More** again, and select **New pipeline**.

    ![On the Author and Deploy blade, the More ellipses is selected, and from its drop-down menu, New pipeline is selected.](media/image156.png "Author and Deploy blade")

2.  Replace the JSON text in the draft window with following JSON.
    ```
    {
        "name": "MLPredictivePipeline",
        "properties": {
            "description": "Use AzureML model",
            "activities": [
                {
                    "type": "AzureMLBatchExecution",
                    "typeProperties": {
                        "webServiceInput": "PartitionedBlobInput",
                        "webServiceOutputs": {
                            "output1": "ScoredBlobOutput"
                        },
                        "webServiceInputs": {},
                        "globalParameters": {}
                    },
                    "inputs": [
                        {
                            "name": "PartitionedBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "ScoredBlobOutPut"
                        }
                    ],
                    "policy": {
                        "timeout": "02:00:00",
                        "concurrency": 10,
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "MLActivity",
                    "description": "prediction analysis on batch input",
                    "linkedServiceName": "AzureMLLinkedService"
                }
            ],
            "start": "2017-03-01T00:00:00Z",
            "end": "2099-12-31T11:59:59Z",
            "isPaused": false,
            "pipelineMode": "Scheduled"
        }
    }
    ```

3.  Select **Deploy**.

    ![Screenshot of the Deploy button.](media/image153.png "Deploy button")

### Task 5: Monitor pipeline activities

1.  Close the Author and Deploy blade, and return to the Data Factory overview.

2.  Select **Monitor & Manage** under **Actions**. ![On the Data Factory blade, under Actions, Monitor and Manage is selected.](media/image157.png "Data Factory blade")

3.  From the Pipelines list select **MLPredictivePipeline.**

    ![The Data factory Resource Explorer is shown and the MLPredictivePipeline has been selected.](media/image158.png "Data factory resource explorer")

4.  Right-click on **MLPredictivePipeline** and then click Open **pipeline**.

    ![The MLPredictivePipeline has been selected and the Open pipeline is shown being clicked.](media/image159.png "MLPredicitivePipeline right-click menu")

5.  Once again, you need to shift the start time in order to see the items in progress and ready states. 
    
    ![The Start time has been changed to 03/01/2017 12:00 am, and the Apply button is selected.](media/image160.png "Start time")

6.  The MLPredictive Pipeline should run and show as Ready.

    ![A view of the Azure Data Factory dashboard, showing each completed run activity.](media/image161.png "Azure data factory dashboard")

7.  Close the **Monitor & Manage** browser tab.

## Exercise 5: Summarize data using HDInsight Spark

Duration: 20 minutes

In this exercise, you will prepare a summary of flight delay data in HDFS using Spark SQL.

### Task 1: Install pandas on the HDInsight cluster

In this task, you will upgrade the version of panda on the HDInsight cluster, to ensure the Jupyter notebook's autovixwidget has the necessary 'api' module installed.

1.  In the Azure portal, navigate to your HDInsight cluster, and from the Overview blade select Secure Shell (SSH). 
    
    ![In the HDInsight cluster blade left menu, Overview is selected. From the top menu, Secure Shell (SSH) is selected.](media/image162.png "HDInsight cluster blade")

2.  On the SSH + Cluster login blade, select your cluster from the Hostname drop down, then select the copy button next to the SSH command. 

    ![In the SSH + Cluster login blade, under Connect to cluster using secure shell (SSH), the copy button next to the SSH command is selected.](media/image163.png "SSH + Cluster login blade")

3.  On your Lab VM, open a new Git Bash terminal window.

4.  At the prompt, paste the SSH command you copied from your HDInsight SSH + Cluster login blade. ![The Get Bash Window displays with the copied SSH command.](media/image164.png "Get Bash Window")

5.  Enter yes, if prompted about continuing, and enter the following password for the **sshuser**:

    a.  **Abc!1234567890**

6.  At the **sshuser** prompt within the bash terminal, enter the following command to install pandas on the cluster. Make sure to wait until the process completes prior to moving on to the next step.
    ```
    sudo -HE /usr/bin/anaconda/bin/conda install pandas -y
    ```

### Task 2: Summarize delays by airport

1.  In the Azure portal (<https://portal.azure.com>), navigate to the blade for your Spark cluster. Do this by going to the resource group you created during the lab setup, using the Resource Group link in the left-hand menu. Once you select your resource group, you will see a list of the resources within that group, including your Spark cluster. Select your Spark cluster.

    ![In the Resource group list, the HDInsight cluster kylespark is selected.](media/image165.png "Resource group list")

2.  In the **Quick links** section, select **Cluster dashboard**.

    ![Cluster dashboard is selected in the Quick links section.](media/image166.png "Quick links section")

3.  From the **Cluster dashboards** blade, select **Jupyter Notebook**.

    ![Jupyter Notebook is selected on the Cluster dashboards blade.](media/image167.png "Cluster dashboards blade")

4.  Juptyer Notebook will open in a new browser window. Log in with the following credentials:

    a.  User name: **demouser**

    b.  Password: **Password.1!!**

Note: If you get a 403 -- Forbidden: Access is denied error, try to open the jupyter URL in a private or incognito browser window. You can also clear the browser cache.

5.  On the Jupyter Notebook screen, select **New**, and **Spark**. This will open a Jupyter notebook in a new browser tab.
    
    ![In the jupyter notebook, the New drop-down button is selected, and from its drop-down menu, Spark is selected.](media/image168.png "jupyter notebook")

6.  Copy the text below, and paste it into the first cell in the Jupyter notebook. This will read the data from our Scored\_FlightsAndWeather.csv file, and output it into a Hive table named "FlightDelays."
    ```
    import spark.sqlContext.implicits._

    val flightDelayTextLines = sc.textFile("/ScoredFlightsAndWeather/*.csv")

    case class AirportFlightDelays(OriginAirportCode:String,OriginLatLong:String,Month:Integer,Day:Integer,Hour:Integer,Carrier:String,DelayPredicted:Integer,DelayProbability:Double)

    val flightDelayRowsWithoutHeader = flightDelayTextLines.map(s => s.split(",")).filter(line => line(0) != "OriginAirportCode")

    val resultDataFrame = flightDelayRowsWithoutHeader.map(
        s => AirportFlightDelays(
            s(0), //Airport code
            s(13) + "," + s(14), //Lat,Long
            s(1).toInt, //Month
            s(2).toInt, //Day
            s(3).toInt, //Hour
            s(5), //Carrier
            s(11).toInt, //DelayPredicted
            s(12).toDouble //DelayProbability
            )
    ).toDF()

    resultDataFrame.write.mode("overwrite").saveAsTable("FlightDelays")
    ```

7.  The notebook should now look like the image below. ![Code displays in the Jupyter notebook.](media/image169.png "Jupyter notebook code")

8.  Select the **Run cell** button on the toolbar. 
    
    ![Screenshot of the Run cell button.](media/image170.png "Run cell button")

9.  You will see in asterisk appear between the brackets in front of the cell.\
    
    ![In the Jupyter notebook code window, the following code is circled: In \[\*\]:](media/image171.png "Jupyter notebook code")

10. This will change to a number once the command is complete.
    
    ![In the Jupyter notebook code window, the circled code has changed to: In \[1\]:](media/image172.png "Jupyter notebook code")

11. Below the cell, you will see the output from executing the command. 

    ![Under Starting Spark application, a table displays with the following columns: ID, YARN Application ID, Kind, State, Spark UI, Driver log, and Current session? ](media/image173.png "Output table")

12. Now, we can query the hive table which was created by the previous command. Paste the text below into the empty cell at the bottom on the notebook, and select the **Run** cell button for that cell.
    ```
    %%sql
    SELECT * FROM FlightDelays
    ```

13. Once completed you will see the results displayed as a table. 

    ![In the Results table has the following columns of information: OriginAirportCode, OriginLatLong, Month, Day, Hour, Carrier, DelayPredicted, and DelayProbability.](media/image174.png "Results table")

14. Next, you will create a table that summarizes the flight delays data. Instead of containing one row per flight, this new summary table will contain one row per origin airport at a given hour, along with a count of the quantity of anticipated delays. In a new cell below the results of our previous cell, paste the following text, and select the **Run** cell button from the toolbar.
    ```
    %%sql
    SELECT  OriginAirportCode, OriginLatLong, Month, Day, Hour, Sum(DelayPredicted) NumDelays, Avg(DelayProbability) AvgDelayProbability 
    FROM FlightDelays 
    WHERE Month = 4
    GROUP BY OriginAirportCode, OriginLatLong, Month, Day, Hour
    Having Sum(DelayPredicted) > 1
    ```

15. Execution of this cell should return a results table like the following. 

    ![This Results table now has the following columns: OriginAirportCode, OriginLatLong, Month, Day, Hour, NumDelays, and AvgDelayProbability.](media/image175.png "Results table")

16. Since the summary data looks good, the final step is to save this summary calculation as a table, which we can later query using Power BI (in the next exercise).

17. To accomplish this, paste the text below into a new cell, and select the **Run** cell button from the toolbar.

    val summary = spark.sqlContext.sql("SELECT  OriginAirportCode, OriginLatLong, Month, Day, Hour, Sum(DelayPredicted) NumDelays, Avg(DelayProbability) AvgDelayProbability FROM FlightDelays WHERE Month = 4 GROUP BY OriginAirportCode, OriginLatLong, Month, Day, Hour Having Sum(DelayPredicted) > 1")
    summary.write.mode("overwrite").saveAsTable("FlightDelaysSummary")

18. To verify the table was successfully created, go to another new cell, and enter the following query.

    %%sql
    SELECT * FROM FlightDelaysSummary

19. Select the **Run cell** button on the toolbar. 
    
    ![Screenshot of the Run cell button.](media/image170.png "Run cell button")

20. You should see a results table similar to the following. ![The same Results Table displays.](media/image176.png "Results table")

21. You can also select Pie, Scatter, Line, Area, and Bar chart visualizations of the dataset.

## Exercise 6: Visualizing in Power BI Desktop

### Task 1: Connect to the Lab VM

1.  If you are already, connected to your Lab VM, skip to Task 2.

2.  From the left side menu in the Azure portal, click on **Resource groups**, then enter your resource group name into the filter box, and select it from the list. 

    ![In the left menu of the Azure Portal, Resource groups is selected. In the Resource groups blade, bigdata is typed in the search field, and in the results below, bigdatakyle is selected.](media/image14.png "Azure Portal")

3.  Next, select your lab virtual machine from the list. 
    
    ![In the Virtual machine list, the kylelab virtual machine is selected.](media/image15.png "Virtual machine list")

4.  On your Lab VM blade, select **Connect** from the top menu. 

    ![The Connect button is selected on the Lab VM blade.](media/image16.png "Lab VM blade")

5.  Download and open the RDP file.

6.  Select Connect, and enter the following credentials:

    a.  User name: **demouser**

    b.  Password: **Password.1!!**

### Task 2: Connect to HDInsight Spark using Power BI Desktop

1.  On your Lab VM, launch Power BI Desktop by double-clicking on the desktop shortcut you created in the pre-lab setup.

2.  When Power BI Desktop opens, you will need to enter your personal information, or Sign in if you already have an account.

    ![The Power BI Desktop Welcome page displays.](media/image177.png "Power BI Desktop Welcome page")

3.  Select Get data on the screen that is displayed next. ![On the Power BI Desktop Sign in page, in the left pane, Get data is selected.](media/image178.png "Power BI Desktop Sign in page")

4.  Select **Azure** from the left, and select **Azure HDInsight Spark (Beta)** from the list of available data sources. 

    ![In the left pane of the Get Data page, Azure is selected. In the right pane, Azure HDInsight Spark (Beta) is selected.](media/image179.png "Get Data page")

5.  Select **Connect**.

6.  You will receive a prompt warning you that the Spark connector is still in preview. Select **Continue**. 
    
    ![A warning reminds you that the app is still under development.](media/image180.png "Preview connector warning")

7.  On the next screen, you will be prompted for your HDInsight Spark cluster URL.
    
    ![The Azure HDInsight Spark Server page displays, and the Server field is empty.](media/image181.png "Server page")

8.  To find your Spark cluster URL, go into the Azure portal, and navigate to your Spark cluster, as you did in Exercise 5, Task 1. Once on the cluster blade, look for the URL under the Essentials section.
    
    ![In the Essentials section, the Spark cluster URL https://kylespark.azurehdinsight.net is selected.](media/image182.png "Essentials section")

9.  Copy the URL, and paste it into the Server box on the Power BI Azure HDInsight Spark dialog.

    ![On the Azure HDInsight Spark Server page, the copied URL is now pasted into the Server field.](media/image183.png "Server page")

10. Select DirectQuery for the Data Connectivity mode, and select **OK**.

11. Enter your credentials on the next screen as follows.

    a.  User name: **demouser**

    b.  Password: **Password.1!!**![The azure/kylespark.azurehdinsight.net sign-in page displays.](media/image184.png "Sign-in page")

12. Select **Connect**.

13. In the Navigator dialog, check the box next to **flightdelayssummary**, and select **Load**.

    ![In the Navigator dialog box, in the left pane under Display Options, the check box for flightdelayssummary is selected. In the right pane, the table of flight delays summary information displays.](media/image185.png "Navigator dialog box")

14. It will take several minutes for the data to load into the Power BI Desktop client.

### Task 3: Create Power BI report

1.  Once the data finishes loading, you will see the fields appear on the far right of the Power BI Desktop client window.

    ![Screenshot of the Fields column.](media/image186.png "Power BI Desktop Fields")

2.  From the Visualizations area, next to Fields, select the Globe icon to add a Map visualization to the report design surface.

    ![On the Power BI Desktop Visualizations palette, the globe icon is selected.](media/image187.png "Power BI Desktop Visualizatoins palette")

3.  With the Map visualization still selected, drag the **OriginLatLong** field to the **Location** field under Visualizations. Then Next, drag the **NumDelays** field to the **Size** field under Visualizations.

    ![In the Fields column, the check boxes for NumDelays and OriginLatLong are selected. An arrow points from OriginLatLong in the Fields column, to OriginLatLong in the Visualization\'s Location field. A second arrow points from NumDelays in the Fields column, to NumDelays in the Visualization\'s Size field.](media/image188.png "Visualizations and Fields columns")

4.  You should now see a map that looks similar to the following (resize and zoom on your map if necessary): 

    ![On the Report design surface, a Map of the United States displays with varying-sized dots over different cities.](media/image189.png "Report design surface")

5.  Unselect the Map visualization by clicking on the white space next to the map in the report area.

6.  From the Visualizations area, select the **Stacked Column Chart** icon to add a bar chart visual to the report's design surface.

    ![The stacked column chart icon is selected on the Visualizations palette.](media/image190.png "Visualizations palette")

7.  With the Stacked Column Chart still selected, drag the **Day** field and drop it into the **Axis** field located under Visualizations.

8.  Next, drag the **AvgDelayProbability** field over, and drop it into the **Value** field. 

    ![In the Fields column, the check boxes for AvgDelayProbability and Day are selected. An arrow points from AvgDelayProbability in the Fields column, to AvgDelayProbability in the Visualization\'s Axis field. A second arrow points from Day in the Fields column, to Daly in the Visualization\'s Value field.](media/image191.png "Visualizations and Fields columns")

9.  Grab the corner of the new Stacked Column Chart visual on the report design surface, and drag it out to make it as wide as the bottom of your report design surface. It should look something like the following. 

    ![On the Report Design Surface, under the map of the United States with dots, a stacked bar chart displays.](media/image192.png "Report Design Surface")

10. Unselect the Stacked Column Chart visual by clicking on the white space next to the map on the design surface.

11. From the Visualizations area, select the Treemap icon to add this visualization to the report. ![On the Visualizations palette, the Treemap icon is selected.](media/image193.png "Visualizations palette")

12. With the Treemap visualization selected, drag the **OriginAirportCode** field into the **Group** field under Visualizations.

13. Next, drag the **NumDelays** field over, and drop it into the **Values** field. 

    ![In the Fields column, the check boxes for NumDelays and OriginAirportcode are selected. An arrow points from NumDelays in the Fields column, to NumDelays in the Visualization\'s Values field. A second arrow points from OriginAirportcode in the Fields column, to OriginAirportcode in the Visualization\'s Group field.](media/image194.png "Visualizations and Fields columns")

14. Grab the corner of the Treemap visual on the report design surface, and expand it to fill the area between the map and the right edge of the design surface. The report should now look similar to the following. 

    ![The Report design surface now displays the map of the United States with dots, a stacked bar chart, and a Treeview.](media/image195.png "Report design surface")

15. You can cross filter any of the visualizations on the report by clicking on one of the other visuals within the report, as shown below. (This may take a few seconds to change, as the data is loaded.) 

    ![The map on the Report design surface is now zoomed in on the northeast section of the United States, and the only dot on the map is on Chicago. In the Treeview, all cities except MDW are grayed out. In the stacked bar graph, each bar is now divided into a darker and a lighter color, with the darker color representing the airport.](media/image196.png "Report design surface")

16. You can save the report, by clicking Save from the File menu, and entering a name and location for the file. 

    ![The Power BI Save as window displays.](media/image197.png "Power BI Save as window")

## Exercise 7: Deploy intelligent web app

Duration: 20 minutes

In this exercise, you will deploy an intelligent web application to Azure from GitHub. This application leverages the operationalized machine learning model that was deployed in Exercise 1 to bring action-oriented insight to an already existing business process.

### Task 1: Deploy web app from GitHub

1.  Navigate to <https://github.com/ZoinerTejada/mcw-big-data-and-visualization/blob/master/AdventureWorksTravel/README.md> in your browser of choice, but where you are already authenticated to the Azure portal.

2.  Read through the README information on the GitHub page.

3.  Click the **Deploy to Azure** button.

    ![Screenshot of the Deploy to Azure button.](media/image3.png "Deploy to Azure button")

4.  On the following page, ensure the fields are populated correctly.

    a.  Ensure the correct Directory and Subscription are selected.

    b.  Select the Resource Group that you have been using throughout this lab.

    c.  Either keep the default Site name, or provide one that is globally unique, and then choose a Site Location.

    d.  Finally, enter the ML API and Weather API information.

Note: Recall that you recorded the ML API information back in Exercise 1, Task 9.

-   This information can be obtained on your Machine Learning web service page (<https://services.azureml.net>, then go to the Consume tab.

-   The Primary Key listed is your ML API key

-   In the Request-Response URL, the GUID after subscriptions/ is your ML Workspace Id

-   In the Request-Response URL, the GUID after services/ is your ML Service Id![Fields in the Basic consumption page display the previously defined settings.](media/image198.png "Basic consumption page")

    e.  Also, recall that you obtained the Weather API key back in the Task 3 of the prerequisite steps for the lab. Insert that key into the Weather Api Key field. 
    
    ![Fields on the Deploy to Azure page are populated with the previously copied information.](media/image199.png "Deploy to Azure page")

5.  Select **Next**, and on the following screen, select **Deploy**.

6.  The page should begin deploying your application while showing you a status of what is currently happening.

Note: If you run into errors during the deployment that indicate a bad request or unauthorized, verify that the user you are logged into the portal with an account that is either a Service Administrator or a Co-Administrator. You won't have permissions to deploy the website otherwise.

7.  After a short time, the deployment will complete, and you will be presented with a link to your newly deployed web application. CTRL+Click to open it in a new tab.

8.  Try a few different combinations of origin, destination, date, and time in the application. The information you are shown is the result of both the ML API you published, as well as information retrieved from the Weather Underground API.

9.  Congratulations! You have built and deployed an intelligent system to Azure.

## After the hands-on lab 

Duration: 10 minutes

In this exercise, attendees will deprovision any Azure resources that were created in support of the lab.

### Task 1: Delete resource group

1.  Using the Azure portal, navigate to the Resource group you used throughout this hands-on lab by selecting **Resource groups** in the left menu.

2.  Search for the name of your research group and select it from the list.

3.  Select **Delete** in the command bar and confirm the deletion by re-typing the Resource group name and selecting **Delete**.

You should follow all steps provided *after* attending the Hands-on lab.

