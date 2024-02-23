**Lift&Shift migration use case In-Place Upgrade & Extended Security Update with Azure Bicep Nested Virtualized Hyper-V Environment to Azure Migrate**

**Agenda**

**Prerequisites**

**Deploy Lab Environment via Azure Bicep for Nested Virtualized Hyper-V Environment for Azure Migration**

**Prepare Azure Migration Service Appliance for Migration**

**Replicate Hyper-V VMs via Azure Migration Tool**

**Migrate Windows Server 2012 OS workloads using Azure Migrate with in-place Upgrade Use Case**

**Prerequisites**

Cloud Accounts

Azure subscription access (Nordcloud Migration Practice tenant), NC Migrations Sandbox subscription; Account in the Nordcloud Azure tenant is a prerequisite.Please request using this form: Account

Nordcloud Bitbucket access, Nordcloud (nordcloud)

Tools on your PC

Visual Studio CodeInstallation: https://code.visualstudio.com/download

Azure CLIInstallation:  

Azure Bicep Extension on VS CodeInstallation: Set up Bicep development and deployment environments - Azure Resource Manager

All the above utils are free and can be downloaded from the Internet

**Deploy a Lab Environment using Azure Bicep for a Nested Virtualized Hyper-V Environment for Azure Migration**

Install Visual Studio Code or any other preferred code editor with git support (or other tool supporting git)

Clone migration-training repository 

Please check the repository’s path in the terminal to ensure your repo is ready in to right place for the deployment. Alternatively, right-click on the Folder (VS Code repository) and click Open in the integrated terminal to choose the relevant path.

Use the Azure CLI command in the terminal to log in to your Azure subscription. Afterward, please follow the commands below;

az login --tenant 9b07d803-edb3-492a-b7f6-82b307c3892elogin to Azure to NC Migrations tenant.

az account set --subscription 866a781f-d26d-484d-baeb-1a4db562a93aselect NC Migrations Sandbox subscription, if you currently have more than one subscription on your account.

az group create --name azmig-workshop-demo<99> --location "westeurope "Create a resource group for the deployment. Please follow naming standards which have been shared before the lab.Replace <99> here and in later commands with a two-digit number shared for you on the workshop.

az deployment group create --resource-group azmig-workshop-demo<99>  --name azmig-workshop-demo<99> --template-file main.bicep --parameters main.parameters.json - the parameters help start the lab environment’s deployment.

The deployment will be completed in about 20-25 minutes. As soon as deployment has successfully finished, please check the relevant resource group and its resources on Azure Portal and please make sure the host server VM’s custom script extension installation is completed.

In your resource group find a virtual machine <prefix>-Host and login using remote desktop (Overview > Connect >  Connect via Azure Bastion) to trigger the logon script. Use user name and password from main.parameters.json file.

Please wait till the Logon Script tasks are completed successfully. This process can take 20-25 minutes.

Please check if the workloads are installed on the Hyper-V host successfully. All deployment tasks will be completed in 40-45 minutes. Open START > Windows Administrative Tools > Hyper-V Manager.

**Prepare Azure Migration Tool  for Migration**
To create an Azure Migrate project: 

→ Search Azure Migrate on the Azure portal → In Servers, databases, and web apps, select Create project. For more detail: https://learn.microsoft.com/en-us/azure/migrate/create-manage-projects#create-a-project-for-the-first-time 
Use name azmig-workshop-demo<99> for the new project.

Click Discover under the Migration tools section in Servers, databases, and web apps.

Choose Hyper-V virtualization and target region to continue;

To prepare the Hyper-V host servers download the AzureSiteRecoveryProvider.exe replication provider using the first blue Link “Download”.Then download the registration key using the blue Button “Download”.

Install the downloaded software installer on the respective Hyper-V host via the registration key file will be downloaded as shared below in the screenshot.

The registration process’s straightforward steps end up ready for the Hyper-V host to replication.

Finalize the registration to start replication VMs located on Hyper-V servers over the Azure Migration tool. This registration finalization process may take up to a couple of minutes.

**Replicate Hyper-V VMs via Azure Migration Tool**
Start the replication process via the replicate button over the Azure migrate tool and specify the type of migration and target migration scope as defined below;



Choose the following scenario “Yes, with Hyper-V” for virtualized workloads.



For the scenario, select a single VM to replicate and prevent latency and bottlenecks.



Please provide the target settings for the resource details created in the previous deployment task.



Follow the steps to complete replication settings for the source VM with the Compute stage. VM name can be changed to be migrated VM if desired. Virtual machine size can be set specifically. Later on, please select OS type.

PS: Note: In the case of the previously configured D&A tool, the assessment gives more specific outputs regarding VM resources such as CPU, I\O, estimated Azure VM size, etc.



Define disk type from in between supported Azure Disk types SSD, Standard HDD, or Premium.


Afterward completing the steps, all replication settings are placed to start the replication with the replicate process. Please click the replicate on the bottom and start the replication for the selected VM/VMs.


When start the replication you will receive a notification that indicates the starting replication server and the replication enabling process like below;

**Migration Steps**

After the initial replication has been done and created first recovery point time, the VM is ready for migration. In this case, we will go through with the migrate to start the migration. 

Although the test failover step isn’t mandotary stage for a migration process, this always has to be considered in the real project or production environment. We will skip this step for the demo but it’s never recommended for the real scenarios.


Once you click the migrate button, have to continue with the opened page that is relevant the prepare the shutdown to avoid data loss or possible issues on the source VM. 


The second thing to consider is the option to upgrade the operating system. Although it's an optional step, we are specifically addressing scenarios where the OS is no longer supported and needs to be migrated. Therefore, based on the presentation and use case scenario, we have chosen the in-place upgrade option for the VM that needs to be migrated.

Please specify the OS version of your source VM before starting the process. Azure Migrate is not yet able to detect the current version automatically. 

As explained during the presentation about the migration support matrix, this step helps to determine which OS versions are suitable for the upgrade. 

Start the migration process after selecting the correct and desired OS versions.


After initiating the migration, a notification is received which allows you to track progress through job steps. Once the VM has been successfully migrated, it will become an Azure resource.

When the migration is completed successfully, check the VM details and OS upgrade version by going to the respective resource over the relevant resource group (Click on  Resource Group → VirtualMachineName) and then verify both process's results.
