# Steps to deploy the function and APIs

- [Import](https://docs.microsoft.com/en-us/azure/devops/repos/git/import-git-repository?view=azure-devops) this repo to an Azure DevOps Repo
- Create two [ARM service connections](https://docs.microsoft.com/en-us/azure/devops/pipelines/library/connect-to-azure?view=azure-devops) each scoped to the apim resource group and the fucntion app resource group
- Create a new [Agent pool](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/pools-queues?view=azure-devops&tabs=yaml%2Cbrowser) with name _apim-cs_

## Deploy the backend

- Create a pipeline using the deploy-function.yml file
- Add [variables](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops&tabs=yaml%2Cbatch#access-variables-through-the-environment) to the pipeline 
    - armServiceConnection - the service connection scoped to the backend resource group
    - functionAppName - name of the funciton app in the backend resource group
    - poolName
- Run the pipeline

## Deploy the APIs

### Generator pipeline

- Create a pipeline using the apim-generator.yml file. This generates the ARM templates from open api specification
- Add variables for
    - poolName
- Run the pipeline    

### Collector pipeline

- Create a pipeline using the apim-collector.yml file. This collects the artifacts from the generator and deploys.
- Add variables for
    - poolName
    - apimResourceGroup
    - apimName
    - todoServiceUrl - url of the function app
    - armServiceConnection - the service connection scoped to the apim resource group
    - teamOneBuildPipelineId - Id of the generator pipleline which can be seen in the url
- Run the pipeline 