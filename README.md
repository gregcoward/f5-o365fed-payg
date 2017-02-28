### F5 Officd 365 SAML Federation Solution Template ###

### Description ###
Microsoft Office 365 is a popular choice when you are looking to outsource the management and infrastructure costs of running commodity applications like email and other productivity tools. Office 365 enables the use of a federated identity model that gives you full control of a user's identity, including their password hashes.

Using F5 BIG-IP with Access Policy Manager, (APM) lets you to provide secure, federated identity management from your existing Active Directory to Office 365, without the complexity of additional layers of Active Directory Federation Services (ADFS) servers and proxy servers. You can use many of the enhanced APM security features, such as geographical restrictions and multi-factor authentication, to further protect access to Office 365.

To properly configure and utilize the F5 o365 federation solution it will be necessary for the BIG-IP to have access to one your organization's domain controllers.  This enables the BIG-IP to pre-authenticate users against the organization's active directory. Typically, this can be accomplished by either establishing secure connectivity, (i.e. IPSec VPN or Express Route) between the Azure environment and the corporate infrastructure or by maintaining a read-only domain controller copy in Azure. The configuration will look like the following diagram:

![screenshot](WAF_1.png)


### F5 BIG-IP instance types and pricing tiers ###

You choose the throughput and corresponding Azure instance based on the number of cores and throughput you need. The instances listed below are minimums; you can choose larger instances if you want.

| Cores | Througput | Minimum Azure Instance |
| --- | --- | --- |
| 2 | 25 Mbps | D2_v2 |
| 4 | 200 Mbps | A3 Standard or D3_v2 |
| 8 | 1 Gbps | A4 or A7 Standard or D4v2 |

### Template parameters ###

| Parameter | Required | Description |
| --- | --- | --- |
| numberOfIntances | x | The number of BIG-IPs that will be deployed in front of your application.  This value is hard coded at 1 or 2, depending on the template you selected. |
| instanceType | x | The desired Azure Virtual Machine instance size. |
| instanceThroughput | x | The desired Azure Virtual Machine instance throughput. The values are 1G, 200M, and 25M. |
| adminUsername | x | A user name to login to the WAFs.  The default value is "azureuser". |
| adminPassword | x | A strong password for the WAFs. Remember this password; you will need it later. |
| dnsLabel | x | Unique DNS Name for the public IP address used to access the WAFs for management. |
| applicationProtocols | x | The protocol that will be used to configure the application virtual servers. The only allowed values for these templates are http, https, or https-offload. |
| applicationAddress | x | The public IP address or DNS FQDN of the application that this WAF will protect. |
| applicationPort | x | The unencrypted port that your application is listening on (for example, 80). This field is required in the http and https-offload deployment scenarios. |
| applicationSecurePort | x | The encrypted port that your application is listening on (for example, 443). This field is required in the https deployment scenario. |
| applicationType | x | The operating system on which your application is running. (Linux OS or Windows OS). |
| blockingLevel | x | The level of traffic you want to flag as insecure. All applications behind the WAF will use this level. The higher the level, the more traffic that is blocked. The lower the level, the more chances that unsecure traffic will make it through to your application. See the Security blocking levels topic for more information. |
| customPolicy |  | The URL of a custom ASM security policy, in XML format, that you would like to apply to the deployment. |
| vaultName | x | The name of the Azure Key Vault where you have stored your SSL cert and key in .pfx format as a secret. This field is required in the https and https-offload deployment scenarios. |
| vaultResourceGroup | x | The name of the Azure Resource Group where the previously entered Key Vault is located. This field is required in the https and https-offload deployment scenarios. |
| secretUrl | x | The public URL of the Azure Key Vault secret where your SSL cert and key are stored in .pfx format. This field is required in the https and https-offload deployment scenarios. |
| certThumbprint | x | The thumbprint of the SSL cert stored in Azure Key Vault. This field is required in the https and https-offload deployment scenarios. |
| restrictedSrcAddress | x | Restricts management access to a specific network or address. Enter a IP address or address range in CIDR notation, or asterisk for all sources. |
| tagValues |  | A list of key-value pairs used to create tags on Azure resources. |

### Results ###

This template will create a new resource group, and inside this new resource group it will configure the following:

* Availability Set
* Azure Load Balancer
* Network Security Group
* Storage Account
* Public IP Address
* Network Interface objects for the F5 SAML federation devices
* F5 SAML Federation Virtual Machines

### Connecting to the management interface of the F5 o365 Federation virtual machine(s) ###

After the deployment successfully finishes, you can find the BIG-IP Management UI\SSH URLs by doing the following: 

* Find the resource group that was deployed, which is the same name as the "dnsNameForPublicIP".  When you click on this object you will see the deployment status.  
* Click on the deployment status, and then the deployment.  
* In the "Outputs" section you will find the URL's and ports that you can use to connect to the F5 WAF cluster. 

### Viewing and clearing security violations ###

From the BIG-IP Management UI, you can view and accept/ignore detected security violations:

* Click Security > Event Logs > Application > Requests
* From the Requests List, select the illegal request you want to view
* Select an action to be performed for future occurences of this violation
* From the top menu, click Apply Policy to apply the changes


### Deploy F5 Office 365 SAML Federation Solution in Azure ###

### Single WAF - Deploys one BIG-IP VE ###
### HTTP - Deploys an unencrypted application service ###
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Ff5devcentral%2Ff5-azure-waf-community%2Fmaster%2Ftemplates%2Fsingle-WAF%2Fhttp%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>