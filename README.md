# azure-ingest-lacework
Custom connector for Azure to ingest Lacework events

author: Dan Daggett 

This custom Azure Logic App connector will setup a Webhook listener. Lacework can then use this webhook to send events to Azure Sentinel under the Lacework_CL datasource.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsgviking%2Fazure-ingest-lacework%2Fmain%2Fazure-ingest-lacework.json)
[![Deploy to Azure Gov](https://aka.ms/deploytoazuregovbutton)](https://portal.azure.us/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsgviking%2Fazure-ingest-lacework%2Fmain%2Fazure-ingest-lacework.json)

## Installing integration  

You will need to first click the above Deploy button to install the Logic App connector for Lacework into Azure.  Once you click the link you will need to specify the resource group, workspace ID, and workspace key.  

![Config screenshop](https://raw.githubusercontent.com/sgviking/azure-ingest-lacework/main/images/logicappconfig.png)

Once the app is in place you can click into Automation, Active Playbooks, and then click on Ingest-Lacework. From there click on Trigger History.  This screen has the URL you will need to configure the webhook alert channel in Lacework.  

![Webhook Screenshot](https://raw.githubusercontent.com/sgviking/azure-ingest-lacework/main/images/webhook_url.png)

Take this URL and go over to the Lacework platform. Add a new Alert Channel of type webhook. You will need to give it a name and the webhook URL from above.  

[Setup Lacework Webhook Alert Channel](https://docs.lacework.com/webhook)  

You can simulate an event by posting the JSON from the above link to the webhook URL.

Last you can view these events in Microsoft Sentinel by going to Logs and then running a query such as this `Lacework_CL | limit 10`.  

![Query Lacework events in Sentinel](https://raw.githubusercontent.com/sgviking/azure-ingest-lacework/main/images/search.png)  

## Other starter queries  

**Last 30 days billable usage** - This query will pull back the last 30 days of usage. If it's just Lacework data this number will be small so showing in MB.  

```
Usage
| where TimeGenerated > startofday(ago(31d))
| where StartTime > startofday(ago(31d))
| where IsBillable == true
| summarize TotalVolumeMB = sum(Quantity) by bin(StartTime, 1d), Solution
| render columnchart
```  

**Lacework events by type**  

```
Lacework_CL
| where event_type_s !contains 'TestEvent'
| summarize event_count = count() by event_type_s
| limit 1000
```  

**Lacework events by title**

```
Lacework_CL
| where event_type_s !contains 'TestEvent'
| summarize event_count = count() by event_title_s
| limit 1000
```  


**Lacework events by source**
```
Lacework_CL
| where event_type_s !contains 'TestEvent'
| summarize event_count = count() by event_
| limit 1000
```  

## Add a Lacework Workbook  

Go into your workspace and then click into Workbooks. Click to add a new workbook.

![Add Workbook](https://raw.githubusercontent.com/sgviking/azure-ingest-lacework/main/images/add_workbook.png)  

Click on Edit and then Click on <> to see the code view.  

![Code View](https://raw.githubusercontent.com/sgviking/azure-ingest-lacework/main/images/code_view.png)

Replace the text in the box with the JSON [from this repository](https://raw.githubusercontent.com/sgviking/azure-ingest-lacework/main/workbooks/lacework_events.json). Click Apply and then click Save.  

This will give you a workbook that looks something like this.  

![Azure Lacework Workbook](https://raw.githubusercontent.com/sgviking/azure-ingest-lacework/main/images/azure_dashboard.png)
