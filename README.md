# Big Data and visualization

AdventureWorks Travel (AWT) provides concierge services for business travelers. In an increasingly crowded market, they are always looking for ways to differentiate themselves, and provide added value to their corporate customers.

They are looking to pilot a web app that their internal customer service agents can use to provide additional information useful to the traveler during the flight booking process. They want to enable their agents to enter in the flight information and produce a prediction as to whether the departing flight will encounter a 15-minute or longer delay, considering the weather forecasted for the departure hour.

## Target audience

- Application developers
- Data scientists
- Data engineers
- Data architects

## Abstract

### Workshop

In this workshop, you will deploy a web app using Machine Learning services to predict travel delays given flight delay data and weather conditions. Plan a bulk data import operation, followed by preparation, such as cleaning and manipulating the data for testing, and training your Machine Learning model.

By attending this workshop, you will be better able to build a complete machine learning model in Azure Databricks for predicting if an upcoming flight will experience delays. In addition, you will learn to:

- Store the trained model in Azure Machine Learning Model Management, then deploy to Docker containers for scalable on-demand predictions

- Use Azure Data Factory (ADF) for data movement and operationalizing ML scoring

- Summarize data with Azure Databricks and Spark SQL

- Visualize batch predictions on a map using Power BI

### Whiteboard Design Session

In this whiteboard design session, you will work with a group to design a solution for ingesting and preparing historic flight delay and weather data, and creating, training, and deploying a machine learning model that can predict flight delays. You will also include a web application that obtains weather forecasts from a 3rd party, collects flight information from end users, and sends that information to the deployed machine learning model for scoring. Part of the exercise will include providing visualizations of historic flight delays, and orchestrating the collection and batch scoring of historic and new flight delay data.

### Hands-on Lab

This hands-on lab is designed to provide exposure to many of Microsoft's transformative line of business applications built using Microsoft big data and advanced analytics. The goal is to show an end-to-end solution, leveraging many of these technologies, but not necessarily doing work in every component possible.

## Azure services and related products

- Azure Databricks
- Azure Machine Learning services
- Azure Data Factory (ADF)
- Azure Storage
- Power BI Desktop
- Azure App Service

## Azure solution

Below is a diagram of the solution architecture you will build in this lab. Please study this carefully so you understand the whole of the solution as you are working on the various components.

![This is the high-level overview diagram of the end-to-end solution.](./Whiteboard-design-session/media/high-level-overview.png 'High-level overview diagram')

# Contributing

This project welcomes contributions and suggestions. Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
