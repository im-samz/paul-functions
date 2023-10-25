# Azure Functions
## Chat Your Email Data using AI Bindings for Functions (C#-Isolated)

This sample shows how to take text documents as a input via BlobTrigger, does Text Summarization processing using the AI Congnitive Language service, and then outputs to another text document using BlobOutput binding.  

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://github.com/codespaces/new?hide_repo_select=true&ref=main&repo=575770869)

## Run on your local environment

### Pre-reqs
1) [.NET 7 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/7.0) required *and [Visual Studio 2022](https://visualstudio.microsoft.com/vs/) is strongly recommended*
2) [Azure Functions Core Tools](https://learn.microsoft.com/en-us/azure/azure-functions/functions-run-local?tabs=v4%2Cmacos%2Ccsharp%2Cportal%2Cbash#install-the-azure-functions-core-tools)
3) [Azurite](https://github.com/Azure/Azurite)

The easiest way to install Azurite is using a Docker container or the support built into Visual Studio:
```bash
docker run -d -p 10000:10000 -p 10001:10001 -p 10002:10002 mcr.microsoft.com/azure-storage/azurite
```

4) Once you have your Azure subscription, [create an Azure Data Explorer instance](https://portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) in the Azure portal to get your key and endpoint. After it deploys, click Go to resource.  Note: if you perform `azd provision` or `azd up` per the section at the end of the tutorial, this resource will already be created.  
You will need the connection string for your Kusto database.  
You will need to create a table in it called `Documents` with the right schema. 

5) Add this local.settings.json file to this folder to simplify local development.  Fill in the empty values.  This file will be gitignored to protect secrets from committing to your repo.  
```json
{
    "IsEncrypted": false,
    "Values": {
        "AzureWebJobsStorage": "UseDevelopmentStorage=true",
        "FUNCTIONS_WORKER_RUNTIME": "dotnet",
        "AZURE_OPENAI_KEY": "",
        "AZURE_OPENAI_ENDPOINT": "https://***.openai.azure.com/",
        "AZURE_OPENAI_SERVICE": "***",
        "AZURE_OPENAI_CHATGPT_DEPLOYMENT": "***",
        "AZURE_OPENAI_DEPLOYMENT": "placeholder",
        "KustoConnectionString": "https://***.eastus2.kusto.windows.net/your-database-here; Fed=true; Accept=true"

    }
}
```

### Using Visual Studio
1) Open `a-bindings.sln` using Visual Studio 2022 or later.
2) Press Run/F5 to run in the debugger

You will see AI analysis happen in the Terminal standard out.  The analysis will be saved in a .txt file in the `` blob container.

### Using Functions CLI
1) Open a new terminal and do the following:

```bash
func start
```

You will see AI analysis happen in the Terminal standard out.  The analysis will be saved in a .txt file in the `test-samples-output` blob container.

## Deploy to Azure

The easiest way to deploy this app is using the [Azure Dev CLI aka AZD](https://aka.ms/azd).  If you open this repo in GitHub CodeSpaces the AZD tooling is already preinstalled.

To provision and deploy:
```bash
azd up
```