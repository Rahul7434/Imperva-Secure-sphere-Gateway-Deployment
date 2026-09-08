# Imperva-Secure-sphere-Gateway-Deployment

For the deployment of Imperva SecureSphere Gateway, first we should have the MX server ready and active license uploaded on MX.
After that, we need to deploy the Gateway VM in Azure. We need to add the required data disk as per sizing requirement and configure static private IP.
Before starting FTL, required connectivity should be allowed. Port 22 is required from jump server to Gateway for SSH. Port 8083 is required for MX GUI access and Gateway communication with MX. Port 443 is required for MX management of Gateway and Agent registration. Port 5555 is the Agent listener port for audit data communication from Agent to Gateway. If Gateway cluster is used, port 7700 is required for Gateway-to-Gateway data sync and port 3792 TCP/UDP is required for Gateway cluster management.

Before FTL, we need the secure password for MX registration and Imperva user password for local Gateway appliance/support access.
Then start FTL on Gateway. During FTL, select Gateway, select Gateway model like MV2500 or MV6500 as per traffic/log sizing, enable Agent Listener SSL if required, enter Agent listener port 5555, enter MX IP, enter secure password for MX registration, enter Imperva support password, enter timezone, and confirm the configuration.

After successful FTL, verify Gateway registration on MX. If HA/cluster is required, configure Gateway cluster by opening required Gateway-to-Gateway ports and running the cluster configuration command with cluster port and interface.




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
FTL Setup
```
During FTL: (First Time login)
Select Gateway.
Select Gateway model, for example:
MV2500
MV6500
Select whether SSL should be enabled. (This option may consume CPU on agent host)
Enter Agent Listener port 5555, for example default/configured port such as 8030 or your organization-approved listener port.
Enter MX/Management Server IP address.
Enter the same secure password used on MX.
Enter Imperva support password.
Enter timezone.
Select true or false "DO you wish to enable sonar only mode?"
Confirm the Parameters and proceed to next, then it will start the configuration.
 Gateway FTL mandatory parameters include component, model type, management IP, secure password, Imperva password, gateway group, agent listener SSL, and agent listener port.


```
Adding New Gateway to existing Gateway Cluster 

To add the gateway to existing cluster, open the "impcfg" it will promt for:

1. Manage SecureSphere management server
2. Manage SecureSphere gateway server
3. manage platform
4. Show changes
5. Discard chnages
6. Save settings
7. Apply settings
8. Quit (Discart not save)


* Select option 2 for manage SecureSphere gateway server then it will prompt for:
      - Perform action (start, stop, etc)
      - Change management server address/password
      - Manage Hardware Security modules
      - Manage remote agents
      - manage interfaces and routes
      - Change virtual gateway model
      - Change Cluster configuration
  (Ent this level, Jump to previous level, Top level, quite)


* Select option 8 for Change Cluster configguration:
      - Select option 1 for "Add cluster configuration" it will prompt for enter cluster port "5559"
      "Do you want to register into cluster group [y/n]" y and enter the cluster group name.
      - Then select option "t" for "top level" and "S" for save the settings.
     
    
      





```

```





