# azure-ingest-lacework
Custom connector for Azure to ingest Lacework events

author: Dan Daggett 

This custom Azure Logic App connector will setup a Webhook listener. Lacework can then use this webhook to send events to Azure Sentinel under the Lacework_CL datasource.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsgviking%2Fazure-ingest-lacework%2Fmain%2Fazure-ingest-lacework.json)
[![Deploy to Azure Gov](https://aka.ms/deploytoazuregovbutton)](https://portal.azure.us/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsgviking%2Fazure-ingest-lacework%2Fmain%2Fazure-ingest-lacework.json)


## Installing integration  

You will need to first click the above Deploy button to install the Logic App connector for Lacework into Azure.  Once you click the link you will need to specify the resource group, workspace ID, and workspace key.

[![Config screenshop](https://raw.githubusercontent.com/sgviking/azure-ingest-lacework/main/images/logicappconfig.png)]

Once the app is in place you can click into Automation, Active Playbooks, and then click on Ingest-Lacework. From there click on Trigger History.  This screen has the URL you will need to configure the webhook alert channel in Lacework.


[![Webhook Screenshot](https://raw.githubusercontent.com/sgviking/azure-ingest-lacework/main/images/webhook_url.png)]
