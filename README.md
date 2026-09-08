# Imperva-Secure-sphere-Gateway-Deployment

For the deployment of Imperva SecureSphere Gateway, first we should have the MX server ready and active license uploaded on MX.
After that, we need to deploy the Gateway VM in Azure. We need to add the required data disk as per sizing requirement and configure static private IP.
Before starting FTL, required connectivity should be allowed. Port 22 is required from jump server to Gateway for SSH. Port 8083 is required for MX GUI access and Gateway communication with MX. Port 443 is required for MX management of Gateway and Agent registration. Port 5555 is the Agent listener port for audit data communication from Agent to Gateway. If Gateway cluster is used, port 7700 is required for Gateway-to-Gateway data sync and port 3792/559 TCP/UDP is required for Gateway cluster management.

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
```
To add the gateway to existing cluster, open the "impcfg" it will promt for:
(We can use impcfg to modify Imperva appliance configuration safely instead of manually editing operating-system or SecureSphere configuration files. The exact submenus can differ depending on whether the appliance is an MX, Gateway, SOM, or DAS,)

1. Manage SecureSphere management server:
(This option manages settings and operations related to the Management Server, also called the MX.)
Examples:
-Start, stop, or restart MX services
-Check or refresh Management Server status
-Manage MX-specific configuration
-Change settings related to Management Server operations
-Configure or modify MX connectivity
-Perform supported MX registration or system-management operations

2. Manage SecureSphere gateway server:
(This is the main option we will use for activities such as Gateway registration, cluster configuration, Agent-listener configuration, and Gateway service operations.)
-Perform Gateway actions
-Change the Gateway name
-Change the Management Server address or password
-Manage Hardware Security Modules
-Manage remote Agents
-Manage Gateway interfaces and routes
-Change the virtual Gateway model
-Change cluster configuration
-Register Gateway
-Unregister Gateway
-Stop Gateway
-Soft restart
-Hard restart
-Refresh Gateway status

3. manage platform
(This option manages the underlying Imperva appliance platform and operating-system-level settings.
It is not limited to the MX or Gateway application. It controls common appliance functions such as networking, users, hostname, time, and other platform settings.)
-Network interfaces
-IP addresses, -Default gateway, -Static routes, -DNS configuration, -Hostname, -Timezone, -NTP/time servers, -Operating-system users, -Password changes, -External authentication,
-Bootloader settings, -Other appliance-level configuration

4. Show changes
(This shows the configuration changes you have selected during the current impcfg session but have not yet applied.
Think of it as a review screen.)

5. Discard chnages
(This removes the pending changes made during the current configuration session.
It returns the staged configuration to the last saved or active state.)

6. Save settings
(This saves the configuration changes you selected.
However, saving does not necessarily mean that the changes are already active. Some changes need to be applied and may cause services to restart or network settings to change.)

7. Apply settings
(This executes the saved changes and makes them active on the appliance.)
Depending on the change, Apply Settings may:
Run generated impctl commands
Stop Gateway services
Unregister the Gateway
Change configuration
Register the Gateway again
Prepare services
Start or restart services
Change networking
Temporarily interrupt connectivity

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
     
* This will prompt for confirmation "C":
     -impctl service stop
     -impctl gateway cluster config --cluster-port-5559 --clusterinterface=eth0
     - impctl gateway register --gateway-group-name="cluster groupname"
     - impctl service start --prepare --transient gateway
     - After confirmation this will apply the change.

 
 * Now Move Gateway to Cluster Through GUI:
     -Go to the Cluster management on mx GUI 
     -Select the gateway and click on move and it will prompt for select cluster group name
     

```

# Imperva SecureSphere MX Deployment:
```
Imperva SecureSphere MX Deployment on Azure
1. Purpose of the MX
The MX, or Management Server, is the central management component of the Imperva SecureSphere environment.

It is used to:
Provide the SecureSphere GUI
Manage Gateways and Agents
Store SecureSphere configuration
Manage policies
Manage audit configuration
Manage alerts and violations
Manage reports
Upload and manage licenses
Manage Gateway groups and clusters
Maintain system-level configuration

The MX must normally be deployed before the Gateways because the Gateway requires a reachable MX during registration.
```

2. MX basic requirements
```
Before deploying an MX, ensure the following are available:
Azure subscription
Resource group
Supported Azure region
VNet and subnet
NSG rules
Supported VM size
Static private IP
Required storage
Valid hostname
DNS and NTP details
Approved administrator passwords
Secure password
System/internal database password
GRUB password, where requested
License file or license entitlement

Unlike Gateway deployment, you cannot require the MX to already exist before deploying the first MX. The MX is normally the first SecureSphere component deployed.
```
3. Azure VM deployment
```
Log in to Azure Portal.
Go to Azure Marketplace.
Search for the supported Imperva DAM image.
Select the appropriate Imperva DAM Marketplace image.
Select the subscription.
Select or create the resource group.
Select the supported region.
Select the VNet and subnet.
Configure the correct NSG.
Select the supported VM size.
Configure a static private IP.
Avoid assigning a public IP unless explicitly required.
Enter a valid hostname.
Configure the OS disk.
Attach the required additional data disk.
Complete VM deployment.
The same Azure DAM image may be used for multiple appliance roles. The component role is selected during FTL
```
5. MX connectivity requirements
```
Before FTL, allow the approved connectivity.
At minimum, the deployment generally requires:
Jump Server → MX TCP 22
Administrator → MX TCP 8083
Gateway ↔ MX Required management/registration ports
DNS ↔ MX DNS port used by the organization
NTP ↔ MX NTP port used by the organization
```

6. Information required before MX FTL
```
Keep the following information ready:
Management interface
Management IP with CIDR
Default gateway
Hostname
DNS server
DNS domain
NTP/time server
Timezone
Root password
CLI administrator username and password
Secure password
Internal system/database password
GRUB password
Appliance model
License details
The FTL wizard can request component role, administrative passwords, network information, hostname, default gateway, timezone, CLI users, and virtual appliance model
```

7. MX FTL setup
```
Log in to the newly deployed VM through SSH or the supported console.
During FTL:
Select the Management Server/MX component.
Select the required virtual appliance model.
Select the management interface.
Enter the management IP with CIDR.
Enter the default gateway.
Enter the hostname.
Enter the timezone.
Enter DNS servers and domain, if prompted.
Enter NTP servers, if prompted.
Configure the root password.
Configure the CLI administrative user.
Enter the secure password.
Enter the internal system/database password.
Enter the GRUB password, if prompted.
Review the configuration.
Save and confirm it.
Allow the FTL process to initialize the MX database and services.
```
8. Important MX passwords
```
Secure password
The secure password establishes protected trust and registration between Imperva components.
You must keep it available for:
Gateway registration
Component communication
Supported registration operations
System password

The MX may request a system password for its internal database during initial setup.
This is different from:
Root password
CLI-user password
SecureSphere GUI admin password
Gateway Imperva password
GUI administrator password

Used to access the MX GUI:
https://<MX-IP>:8083
```
# Imperva SecureSphere SOM Deployment
```
SOM means Security Operations Manager. In newer Imperva DAM documentation, it may be referred to as Management Server Manager.
SOM is a centralized management layer used when an organization has multiple Management Servers, or MXs.

Main purpose of SOM

SOM is used for:

-Centralized visibility across multiple MX servers
-Managing multiple DAM deployments
-Consolidated enterprise-level monitoring
-Viewing alerts and events from different MX systems
-Centralized reporting
-Managing deployments across multiple sites or regions
-Providing a common operational view to SOC teams
-Monitoring registered MX health and communication

Simple difference:
SOM manages multiple MX servers.
MX manages Gateways, Agents, policies, audit data and alerts.
Gateway receives database activity from Agents.

```
2. Basic requirements
```
Before deploying SOM, we should have:
Approved SOM architecture
SOM or Management Server Manager license
Azure subscription and resource group
Supported Azure region
VNet and subnet
Static private IP
Valid hostname/FQDN
Correct Azure VM sizing
Required disk capacity
Azure NSG and firewall rules
DNS and NTP details
Secure administrative passwords
Details of MX servers that will be registered
Compatible DAM versions on SOM and MX
Maintenance/change window
Jump-server connectivity to SOM

If this is a fresh deployment, MX servers can be deployed before or after SOM. However, the MX must be available before you can complete registration and communication validation between SOM and MX.

For an existing environment, confirm that SOM and MX versions and patches are compatible before registration.
```
