Azure AI services supports Microsoft Entra ID authentication, enabling you to grant access to specific service principals or managed identities for apps and services running in Azure.

There are different ways you can authenticate against Azure AI services using Microsoft Entra ID, including:

# Authenticate using service principals

The overall process to authenticate against Azure AI services using service principals is as follows:

#### Create a custom subdomain

You can create a custom subdomain in different ways including through the Azure portal, Azure CLI, or PowerShell.

For example, you can create a subdomain using PowerShell in the Azure Cloud Shell. To do this, you select your subscription using the following command:

```PowerShell
Set-AzContext -SubscriptionName <Your-Subscription-Name>
```

Then, you create your Azure AI services resource specifying a custom subdomain by running the following:

```PowerShell
$account = New-AzCognitiveServicesAccount -ResourceGroupName <your-resource-group-name> -name <your-account-name> -Type <your-account-type> -SkuName <your-sku-type> -Location <your-region> -CustomSubdomainName <your-unique-subdomain-name>
```

Once created, your subdomain name will be returned in the response.

#### Assign a role to a service principal

You've created an Azure AI resource that is linked with a custom subdomain. Next, you assign a role to a service principal.

To start, you'll need to register an application. To do this, you run the following command:

```PowerShell
$SecureStringPassword = ConvertTo-SecureString -String <your-password> -AsPlainText -Force

$app = New-AzureADApplication -DisplayName <your-app-display-name> -IdentifierUris <your-app-uris> -PasswordCredentials $SecureStringPassword
```

This creates the application resource.

Then you use the **New-AzADServicePrincipal** command to create a service principal and provide your application's ID:


```PowerShell
New-AzADServicePrincipal -ApplicationId <app-id>
```

Finally, you assign the **Cognitive Services Users** role to your service principal by running:


```PowerShell
New-AzRoleAssignment -ObjectId <your-service-principal-object-id> -Scope <account-id> -RoleDefinitionName "Cognitive Services User"
```

# Authenticate using managed identities

Managed identities come in two types:

- **System-assigned managed identity**: A managed identity is created and linked to a specific resource, such as a virtual machine that needs to access Azure AI services. When the resource is deleted, the identity is deleted as well.
- **User-assigned managed identity**: The managed identity is created to be useable by multiple resources instead of being tied to one. It exists independently of any single resource.

You can assign each type of managed identity to a resource either during creation of the resource, or after it has already been created.

For example, suppose you have a virtual machine in Azure that you intend to use for daily access to Azure AI services. To enable a system-assigned identity for this virtual machine, first you make sure your Azure account has the [Virtual Machine Contributor role](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles). Then you can run the following command using Azure CLI in the Azure Cloud Shell terminal:

```Azure Cli
az vm identity assign -g <my-resource-group> -n <my-vm>
```

Then you can grant access to Azure AI services in the Azure portal using the following:

1. Go to the Azure AI services resource you want to grant the virtual machine's managed identity access.
    
2. In the overview panel, select **Access control (IAM)**.
    
3. Select **Add**, and then select **Add role assignment**.
    
4. In the Role tab, select **Cognitive Services Contributor**.![[Pasted image 20250107155630.png]]
5. In the Members tab, for the Assign access to, select **Managed identity**. Then, select **+ Select members**.![[Pasted image 20250107155646.png]]
6. Ensure that your subscription is selected in the Subscription dropdown. And for Managed identity, select **Virtual machine**.
    
7. Select your virtual machine in the list, and select **Select**.
    
8. Finally, select **Review + assign** to review, and then **Review + assign** again to finish.