# Advanced Linux Ansible Template : Deploy N Linux VMs and manage them with Ansible

<a href="https://azuredeploy.net/" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>


This advanced template deploys N number of Linux VMs and it configures Ansible so you can easily manage all the VMS using ansible. Don't suffer more pain configuring and managing all your VMs , just use Ansible! Ansible is a very powerful masterless configuration management system based on SSH.

This template  deploys  N number of Storage Account, a Virtual Network, an Availability Sets (up to 3 Fault Domains and up to 20 Update Domains), one private NIC per VM, one public IP ,a Load Balancer and you can specify SSH keys to access your VMS remotely from your latop.
You will need an additional certificate / public key for the Ansible configuration and they have to be uploaded to a Private storage account.  

The template uses two Custom Scripts  :
-The first script configures SSH keys (public) in all the VMs for the Root user so you can manage the VMS with ansible.
-The second script install ansible on a A1 VM so you can use it as a controller and it also deploys the certificate to /root/.ssh./.
Before you execute the script, you will need to create a PRIVATE storage account and upload your  certificate and public keys used by ansible, as well as the bash scripts (An alternative for the certificates / keys is to use the KeyVault)

Below are the parameters that the template expects

| Name   | Description    |
|:--- |:---|
| location  | Location for all ths services |
| storageAccountName  | Name of the storage account , the template will also append the name of the resource group |
| storageAccountType  | Standard_LRS or Premium_LRS |
| vmNumberOfDataDisks | Number of Data Disk (* For future versions, today a fixed number of 2 disks will be created) |
| vmSizeDataDisks  | Size of Data disks : By default two data disks will be created |
| vmFileSystem | ext4 or xfs (* For future versions) |
| createRAID | True or False. Specify true if you want to RAID all the data disks (* For future versions)  |
| vmSize | Size of VMs |
| numberOfVms | Number of VMS |
| adminUserName | Admin User Name |
| adminPassword | Admin Password |
| sshKeyData | SSH Key data |
| faultDomainCount | Number of Fault domains (Default 3, Maximum :3) |
| updateDomainCount | Number of Update domains (Default : 10 , Maximun : 20) |
| customScriptConfigStorageAccountName |  Storage account name for the Private account that will contain your SSH Keys for ansible and the bash scripts ( Only use a Private storage account, as ssh keys should only be accesible by trusted users) |
| customScriptConfigStorageAccountKey | Storage account Key  |
| sshRootCerBlobLocation | The Certificate for the ssh configuration used by ansible (You will need to upload your certificates / keys to the storage account before executing the template) |
| sshRootPubBlobLocation| The public key for the ssh configuration used by ansible (You will need to upload your keys to the storage account before executing the template)|
| virtualNetworkName| Virtual Network Name|
| dnsNameLabel | DNS Name that wil be associated to the Load balancer|


##Known Issues and Limitations
- Fixed number of data disks (This is due to a current template feature limitation and is fixed at 2 in order to all A0 instances for testing)
- Only two VM instances are added to the load balancer and only them  are accessible via SSH (The Ansible Controller and a second VM)
- Scripts are not yet idempotent and cannot handle updates (This currently works for create ONLY)
- Linux Storage configuration is not yet implemented.
- Ubuntu will be implemented in future versions. Currently only CentOS 6.5 has been fully tested.
- Current version doesn't use secured endpoints. If you are going to host confidential data make sure that you secure the VNET by using Security Groups.