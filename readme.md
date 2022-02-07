# composite-azure-deploy-webapp

This a composite action wrapping the following functionality

1. Render environment specific variables with jnus/json-variables
2. Substitute variables in configuration with microsoft/substitute-variables
3. Deploy artifacts to an Azure web app with azure/webapps-deploy

## Usage

Reference the composite action with a full sha like the following to avoid breaking changes in the repo:
```yaml
segesdk/composite-azure-deploy-webapp@0914465681697fe709557a118d94525546
```
or like the following, to always use latest version by referencing @main

```yaml
segesdk/composite-azure-deploy-webapp@main
```

## Parameters

| Input variable | Description |
|----------|----------|
| publishPath | Publishing path of the build artifacts |
| environment | Target environment to deploy to. Used for rendering environment variables and deployment to Azure
|variableFile| Full path to json file with variables for rendering with jnus/json-variables|
|secrets|Serialized json string for github secrets, used to render environment variables. Syntax: `'${{ toJson(secrets) }}'`|
|configurationFile|Full path to configuration file to perform variable substitution|
|webAppName|Name of the target Azure web app|
|azureCredentials|Service principal credentials|

## Usage example
```yaml
- id: Deployment
       uses: segesdk/composite-azure-deploy-webapp@0914465681697fe709557a118d945255464c9626
       with:
         publishPath: ${{env.PUBLISH_PATH}}
         environment: Dev
         variableFile: '${{env.PUBLISH_PATH}}/variables.json'
         secrets: '${{ toJson(secrets) }}'
         configurationFile: '${{env.PUBLISH_PATH}}/appsettings.json'
         webAppName:  ${{env.PROJECT_NAME}}-dev-api-wa
         azureCredentials: ${{ secrets.AZURE_SEGES_SYSUDV_UDV_SECRET }}
```

## Gotchas
https://confluence.seges.dk/display/SYS/How+to+do+variable+substitution+in+GitHub
