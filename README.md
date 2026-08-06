# Imperva-Secure-sphere-Gateway-Deployment


## Imperva SecureSphere Gateway Deployment on Azure
1. Basic Requirement
```
For deploying Imperva SecureSphere DAM Gateway, first we should have:
-MX/Management Server already deployed.
-Active license uploaded on MX.
-Gateway license capacity should be available.
-Azure VM should be deployed for Gateway.
-Additional disk should be added as per requirement, minimum 500 GB recommended in the Azure guide.
```

3. Azure VM Deployment
```
First deploy the Gateway VM from Azure Marketplace using Imperva DAM image.
-Login to Azure Portal.
-Go to Marketplace.
-Search for Imperva.
-Select Imperva Database Activity Monitoring DAM image.
-Create VM using the required subscription, resource group, region, VNet, and subnet.
-Use a valid hostname/FQDN. The Azure guide notes hostname rules such as second character cannot be a dot and after a dot there must be a letter character.
-Avoid creating a public IP unless strictly required.
-Assign the VM to the correct NSG/subnet.
-Select proper VM size based on your DAM sizing requirement.
-Complete VM deployment.
 The same Azure image is used for both Management Server and Gateway, and during First Time Login you select the component type.
-Attach additional disk.
```








