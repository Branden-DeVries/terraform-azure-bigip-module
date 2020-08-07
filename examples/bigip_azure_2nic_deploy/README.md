## Deploys F5 BIGIP Azure Cloud

This Terraform module deploys 2-NIC BIGIP in Azure with the following characteristics:

Bigip 2 Nic with management and external interface associated with user provided subnet and security-group
  
A random generated password for login to bigip ( Default value of az_key_vault_authentication is false )
  
## Example Usage

Below are the input parameters required for 1 NIC bigip module to deploy in AZURE

```
#Example 2-NIC Deployment
module bigip {
  source                    = "../.."
  dnsLabel                  = "bigip-azure-2nic"
  resource_group_name       = "testbigip"
  mgmt_subnet_id            = [{"subnet_id" = "subnet_id_mgmt" , "public_ip" = true}]
  mgmt_securitygroup_id     = ["securitygroup_id_mgmt"]
  external_subnet_id        = [{"subnet_id" =  "subnet_id_external", "public_ip" = true }]
  external_securitygroup_id = ["securitygroup_id_external"]
  availabilityZones         = var.availabilityZones
}

```

#### Required Input Variables

These variables must be set in the module block when using this module.

`dnsLabel/prefix` (string)

`Description:` This value is inserted in the beginning of each Azure object. Note: requires alpha-numeric without special character

`resource_group_name` (string)

`Description:` The name of the resource group in which the resources will be created

`mgmt_subnet_ids` (List of Maps)

`Description:` Map with Subnet-id and public_ip as keys for the management subnet

`mgmt_securitygroup_ids` (List)

`Description:` securitygroup_ids for the management interface

`availabilityZones` (List)

`Description:` availabilityZones 


#### Optional Input Variables

These variables have default values and don't have to be set to use this module. You may set these variables to override their default values.


`f5_username` (string)

`Description:` The admin username of the F5   Bigip that will be deployed

`Default:` bigipuser

`f5_instance_type` (string)

`Description:` Specifies the size of the   virtual machine 

`Default:` Standard_DS3_v2 

`f5_image_name` (string)

`Description:` 5 SKU (image) to you want to   deploy. Note: The disk size of the VM will be determined based on the option   you select. Important: If intending to provision multiple modules, ensure the   appropriate value is selected, such as AllTwoBootLocations or AllOneBootLocation.

`Default:` 

`f5_version` (string)

`Description:` It is set to default to use the   latest software.

`Default:` latest

`f5_product_name` (string)

`Description:` Azure BIG-IP VE Offer.

`Default:` f5-big-ip-best

`storage_account_type` (string)

`Description:` Defines the type of storage   account to be created. Valid options are Standard_LRS, Standard_ZRS,   Standard_GRS, Standard_RAGRS, Premium_LRS

`Default:` Standard_LRS

`allocation_method:` (string)

`Description:` Defines how an IP address is   assigned. Options are Static or Dynamic

`Default:` Dynamic

`enable_accelerated_networking` (bool)

`Description:` Enable accelerated   networking on Network interface

`Default:` FALSE

`enable_ssh_key` (bool)

`Description:` Enable ssh key   authentication in Linux virtual Machine

`Default:` TRUE

`f5_ssh_publickey` (string)

`Description:` Path to the public key to be   used for ssh access to the VM. Only used with non-Windows vms and can be left   as-is even if using Windows vms. If specifying a path to a certification on a   Windows machine to provision a linux vm use the / in the path versus backslash.   e.g. c:/home/id_rsa.pub

`Default:` ~/.ssh/id_rsa.pub

`doPackageUrl` (string)

`Description:` URL to download the BIG-IP   Declarative Onboarding module

`Default:` latest

`as3PackageUrl` (string)

`Description:` URL to download the BIG-IP   Application Service Extension 3 (AS3) module

`Default:` latest

`tsPackageUrl` (string)

`Description:` URL to download the BIG-IP  Telemetry Streaming module

`Default:` latest

`fastPackageUrl` (string)

`Description:` URL to download the BIG-IP FAST   module

`Default:` latest

`cfePackageUrl` (string)

`Description:` URL to download the BIG-IP   Cloud Failover Extension module

`Default:` latest

`libs_dir` (string)

`Description:` Directory on the BIG-IP to download the A&O Toolchain into

`Default:` /config/cloud/azure/node_modules

`onboard_log` (string)

`Description:` Directory on the BIG-IP to store the cloud-init logs

`Default:` /var/log/startup-script.log

`azure_secret_rg:` (string)

`Description:` The name of the resource group in which the Azure Key Vault exists

`Default:` ""

`az_key_vault_authentication:` (string)

`Description:` Whether to use key vault to pass authentication

`Default:` false

`azure_keyvault_name:` (string)

`Description:` The name of the Azure Key Vault to use

`Default:` ""

`azure_keyvault_secret_name:` (string)

`Description:` The name of the Azure Key Vault secret containing the password

`Default:` ""


#### Output Variables

`mgmtPublicIP:`

`Description:` The actual ip address allocated for the resource

`mgmtPublicDNS:`

`Description:` fqdn to connect to the first vm provisioned

`mgmtPort:`

`Description:` Mgmt Port

`f5_username:`

`Description:` Bigip username

`bigip_password:`

`Description:` Bigip Password (if dynamic_password is choosen it will be random generated password or if azure_keyvault is choosen it will be key vault secret name )

