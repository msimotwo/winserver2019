# vmupgradeazure
<p align="center">
<img src="https://i.imgur.com/hbO1sQ3.png" alt="Azure Logo"/>
</p>

<h2>How To Upgrade Existing Microsoft VM in Azure</h2>

Hello, my name is Meshach Simotwo and I am a IT Professional. Today I will be teaching you how to upgrade an existing Microsoft server on Azure. I will be using [Learn.Microsoft.Com](https://learn.microsoft.com/en-us/azure/virtual-machines/windows-in-place-upgrade) to help me give a detailed step by step for upgrading.<br>

*<h3>If you do not have a Microsoft Azure subscription and are looking to obtaining one, check out my repository on it!. [How to get a Microsoft Azure Subscription](https://github.com/msimotwo/mszuresubscription)</h3>*

<h2>-Environments and Technologies Used-</h2>

- Microsoft Azure
- PowerShell

<h2>-Quick How-To on making Resource Groups-</h2>

  - Making Resource Group's and Virtual Machines in Microsoft Azure!
    - Create a Resource Group and name it "VMsetup"
    - Create an Azure Virtual Machine Windows 10, 4 vCPUs
      - Name: "VMSetup1"
        - Username: "labuser" (for example/whatever you chose)
        - Password: "VMSetup123!1" (for example/whatever you chose)

<p align="center">
<img src="https://i.imgur.com/OIz9zEZ.png" alt="Login"/>
</p>

*<h3>Before you do an upgrade on a VM, review the upgrade requirements.</h3>*

<h2>Windows Versions that are not able for in-place upgrade</h2>
  
  - Windows Server 2012 Datacenter
  - Windows Server 2012 Standard
  - Windows Server 2008 R2 Datacenter
  - Windows Server 2008 R2 Standard

*<h3>A quick message from learn.microsoft.com</h3>*

*- The upgrade media provided by Azure requires the VM to be configured for Windows Server volume licensing. This is the default behavior for any Windows Server VM that was installed from a generalized image in Azure. If the VM was imported into Azure, then it may need to be converted to volume licensing to use the upgrade media provided by Azure. To confirm the VM is configured for volume license activation follow these steps to configure the appropriate KMS client setup key. If the activation configuration was changed, then follow these steps to verify connectivity to Azure KMS service.*
  
<h2>Instructions for upgrading</h2>
 
- The in-place upgrade of the VM requires the managed disks. Its okay, most VM's on Azure use managed disks. Managed disks are like the physical disk but on a virtual level.
- Windows recommends capturing a quick snapshot of the operating system disk and any other data disks on board. *This will enable you to revert to the previous state of the VM if anything fails during the in-place upgrade process.*

- To start an in-place upgrade the upgrade media must be attached to the VM as a Managed Disk. To create the upgrade media, modify the variables in the following PowerShell script for Windows Server 2022. The upgrade media disk can be used to upgrade multiple VMs, but it can only be used to upgrade a single VM at a time. To upgrade multiple VMs simultaneously multiple upgrade disks must be created for each simultaneous upgrade.
- Here is the [Link](https://docs.google.com/document/d/1u3_zoOUYfd8HIRGuitxRaUUYFao06YZ9O-6I9rl8kOs/edit?usp=sharing) for the PowerShell script. Easy copy and paste.

- Attach the upgrade media for the target Windows Server version to the VM which will be upgraded. This can be done while the VM is in the running or stopped state.

<h2>Portal Instructions</h2>

- Sign in to the Azure portal.
  - Search for and select Virtual machines.

    - Select a virtual machine to perform the in-place upgrade from the list.

      - On the Virtual machine page, select Disks.

      - On the Disks page, select Attach existing disks.

        - In the drop-down for Disk name, select the name of the upgrade disk created in the previous step.

          - Select Save to attach the upgrade disk to the VM.

<h3>Time to perform the Upgrade!</h3>

- Heads up! To perform an upgrade, the VM must be in a running state. To check, you can access Azure and click on Virtual Machine to see the state of the VM.
  
  - *Connect to the VM using RDP or RDP-Bastion.*

  - *Determine the drive letter for the upgrade disk (typically E: or F: if there are no other data disks).*

  - *Start Windows PowerShell.*

  - *Change directory to the only directory on the upgrade disk.*

  - *Execute the following command to start the upgrade:*
    - .\setup.exe /auto upgrade /dynamicupdate disable 

  - *Select the correct "Upgrade to" image based on the current version and configuration of the VM using the following table:*

<p align="center">
<img src="https://i.imgur.com/zgXdJnC.png" alt="Table Diagram"/>
</p>

- *During the upgrade process the VM will automatically disconnect from the RDP session. After the VM is disconnected from the RDP session the progress of the upgrade can be monitored through the screenshot functionality available in the Azure portal.*

<h2>Post Upgrade Tips</h3>

- *Delete the snapshots of the OS disk and data disk(s) if they were created.*

- *Delete the upgrade media Managed Disk.*

- *Enable any antivirus, anti-spyware or firewall software that may have been disabled at the start of the upgrade process.*
