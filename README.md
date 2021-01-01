# Azure Logic App: Long-Term Github Stats

Github only tracks 14 days worth of traffic data for each repository. There are a few other projects out there that use C#, Python and other langauges to pull the data locally but I wanted to run this serverless in Azure which is why I created this solution. 

This ARM template will deploy the following: 

- Resource Group
- KeyVault (to store your Github Personal Access Tolken as a Secret)
- Storage Account (to store your Github stats as Table Storage)
- Logic App 

The Logic App is triggered daily, and pulls the PAT from the key vault, queries the Github API, parses the JSON, and stores it in Table Storage. 

After the data is loaded into Table Storage you can do whatever you'd like with it, though i've included a basic PowerBI template to visualize the data using PowerBI desktop.

This solution should cost less than $0.05/month (I'll update this statistic once I've run it longer, as of Dec 31, 2020 I've had it running for 4 days and it's cost me $0.01). 


## How to deploy: 

- Step 1: 
  - You will need a Github Personal Access Tolken, you can view the Github docs on that here: https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token
  - Click "Deploy to Azure" below
  - Open CloudShell, and run ```get-azaduser -UserPrincipalName yourUserName@company.com | select-object Id``` and copy that Object ID 
  - Enter the parameter values on the deployment screen, and click deploy. 
  - Once complete, you will need to open the API connection for keyvault, go to "Edit API" and click the blue bar that says "Authorize". You'll be presented with a pop-up sign-in box for a one-time authentication to link the connector to KeyVault. Click save after you're done. 
  - Open the Logic App, and click "Run Trigger" (the first run will have failed becuase the connector wasn't authorized so you'll see that in the run history). 
  - You can verify the data by going to the storage account and viewing the table storage using Storage Explorer. 
  - Open the PowerBI template, enter your storage account name and account key (account keys tab on the storage account in the portal) and your data should show up on the dashboard. 

[![Deploy To Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMattHansen0%2Flogicapp-githubstats%2Fmain%2Fazuredeploy.json)  [![Visualize](http://armviz.io/visualizebutton.png)](http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2FMattHansen0%2Flogicapp-githubstats%2Fmain%2Fazuredeploy.json)
