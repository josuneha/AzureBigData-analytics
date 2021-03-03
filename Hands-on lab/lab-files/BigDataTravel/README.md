# Margie's Travel Intelligent App Deployment

## Part of the Microsoft Cloud Workshops

[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmicrosoft%2FMCW-Big-data-and-visualization%2Fmaster%2FHands-on%2520lab%2Flab-files%2FBigDataTravel%2Fazuredeploy.json)

This GitHub repo exists to deploy the web application that is part of the _Big Data & Visualization Hands-on Lab_. We are leveraging a capability of Azure called ARM templates which allow you to specify what your solution looks like from a deployment perspective simply by using JSON code. This is a fairly simple use of ARM templates, but you can actually deploy very complex topologies using this technology - straight from source control. Pretty cool!!

You will need to have completed the requisite exercises in order to deploy this web application to your Azure subscription. You will also need to have created your free developer account at <https://openweathermap.org/home/sign_up> and retrieve your developer API key.

Once you have gathered the following information, you are ready to click the "Deploy to Azure" button at the top/bottom of this page.

- Your Azure ML web service URL
- Your [OpenWeather API key](https://openweathermap.org/home/sign_up)

After clicking the button, you will see a screen where you will need to provide the above information as well as information needed to by Azure to deploy such as:

- The target subscription
- The target resource group (choose existing or create a new one)
- Azure ML web service URL
- OpenWeather API key

After the web app deployment is completed, click "Go to resource group" and once there, click the resource named _webapp-\<guid\>_ and in its own pane, click the "Browse" button around the top-left corner.

Congratulations!

![The Margie's Travel web app is displayed.](images/webapp.png 'Azure Deployment GUI')

[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmicrosoft%2FMCW-Big-data-and-visualization%2Fmaster%2FHands-on%2520lab%2Flab-files%2FBigDataTravel%2Fazuredeploy.json)
