# Install SDOH Portal

[![Deploy To Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fecom-inc%2Fsdoh-deployment%2Fmain%2Fdefault.deployment.manifest.json)  [![Visualize](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/visualizebutton.svg?sanitize=true)](http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fecom-inc%2Fsdoh-deployment%2Fmain%2Fdefault.deployment.manifest.json)

This template deploys a SDOH Portal to Azure App Service. It deploys Postgress server and SDOH Portal docker image.

## Prerequisite
- Github credentials (user and personal access token) with permission to pull docker image (https://particule.io/en/blog/cicd-github-registry)
- Azure subscription (https://azure.microsoft.com/en-us/free)
- Auth0 tenant (https://auth0.com/docs/get-started/learn-the-basics)
- Google api key to use maps (https://developers.google.com/maps/documentation/android-sdk/get-api-key)

## Deployment parameters
- **Name** - main deployment name which is used as prefix for
  - _default dns name_ - used to browse aplication
  - _azure app name_ - used internally inside azure management portal
  - _azure app service plan name_ - used internally inside azure management portal
  - _postgress server name_ - used to access db from SDOH Portal or PowerBI
  - _postgres database name_ - used to access db from SDOH Portal or PowerBI
- **Container Image** - full address of SDOH portal docker image (f.e. ghcr.io/ecom-inc/sdoh/portal:main)
- **Container Registry User** - username with permission to pull docker image 
- **Container Registry Password** - user's password
- **Database Size** - determines amount of CPU and Memory used by database's virtual machine
- **Database Administrator Login** - administrator user name
- **Database Administrator Password** - administrator user password
- **Auth Domain** - auth0 domain, used for SDOH Portal user authentication process
- **Auth Client Id** - auth0 key, used for SDOH Portal user authentication process
- **Google Api Key** - key used to access google's map api 


## Auth0 configuration
Several components should be created for SDOH Portal
### Applications -> Single page application
- Name: SDOH Portal
- Application Login URI: https://###sdoh-portal-public-domain### (f.e. https://sdoh-prod-portal.azurewebsites.net)
- Allowed Callback URLs: https://###sdoh-portal-public-domain###/authenticate (f.e. https://sdoh-prod-portal.azurewebsites.net/authenticate)
- Allowed Logout URLs: https://###sdoh-portal-public-domain### (f.e https://sdoh-prod-portal.azurewebsites.net)
- Allowed Web Origins: https://###sdoh-portal-public-domain### (f.e https://sdoh-prod-portal.azurewebsites.net)
- Connections: Username-Password-Authentication should be on
### APIs -> Custom API
- Name: sdoh-portal-api
- Identifier: https://sdoh/portal-api
- Permissions: "be:administrator", "be:contributor", "be:reader"
### Users
- any user with permission "be:administrator"

## Postgress options
- version: 11
- ssl enforcement: Disabled
- allowed access from all azure resources
- database sizes mapping (https://docs.microsoft.com/en-us/azure/azure-sql/database/resource-limits-vcore-single-databases): 
  - normal -> GP_Gen5_2
  - medium -> GP_Gen5_8
  - large -> GP_Gen5_16

## Azure App Service options
- http 2: Enabled
- minTlsVersion: 1.2
- http to https redirect: Enabled
- Pricing plan (https://azure.microsoft.com/en-us/pricing/details/app-service/windows/): B1
