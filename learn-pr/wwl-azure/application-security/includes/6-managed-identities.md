
A common challenge when building cloud applications is how to manage the credentials in your code for authenticating to cloud services. Keeping the credentials secure is an important task. Ideally, the credentials never appear on developer workstations and aren't checked into source control. Azure Key Vault provides a way to securely store credentials, secrets, and other keys, but your code has to authenticate to Key Vault to retrieve them.

**Managed Identities** for Azure resources is the new name for the service formerly known as Managed Service Identity (MSI) for Azure resources feature in Microsoft Entra solves the above noted problem. The feature provides Azure services with an automatically managed identity in Microsoft Entra ID. You can use the identity to authenticate to any service that supports Microsoft Entra authentication, including Key Vault, without any credentials in your code.

The managed identities for Azure resources feature is free with Microsoft Entra ID for Azure subscriptions. There's no additional cost.

### Terminology

The following terms are used throughout the managed identities for Azure resources documentation set:

 -  **Client ID** \- a unique identifier generated by Microsoft Entra ID that is tied to an application and service principal during its initial provisioning.
 -  **Principal ID** \- the object ID of the service principal object for your managed identity that is used to grant role-based access to an Azure resource.
 -  **Azure Instance Metadata Service (IMDS)** \- a REST endpoint accessible to all IaaS VMs created via the Azure Resource Manager. The endpoint is available at a well-known non-routable IP address (169.254.169.254) that can be accessed only from within the VM.

### How managed identities for Azure resources works

There are two types of managed identities:

 -  **A system-assigned managed identity** is enabled directly on an Azure service instance. When the identity is enabled, Azure creates an identity for the instance in the Microsoft Entra tenant that's trusted by the subscription of the instance. After the identity is created, the credentials are provisioned onto the instance. The lifecycle of a system-assigned identity is directly tied to the Azure service instance that it's enabled on. If the instance is deleted, Azure automatically cleans up the credentials and the identity in Microsoft Entra ID.
 -  **A user-assigned managed identity** is created as a standalone Azure resource. Through a create process, Azure creates an identity in the Microsoft Entra tenant that's trusted by the subscription in use. After the identity is created, the identity can be assigned to one or more Azure service instances. The lifecycle of a user-assigned identity is managed separately from the lifecycle of the Azure service instances to which it's assigned.

Internally, managed identities are service principals of a special type, which are locked to only be used with Azure resources. When the managed identity is deleted, the corresponding service principal is automatically removed. Also, when a User-Assigned or System-Assigned Identity is created, the Managed Identity Resource Provider (MSRP) issues a certificate internally to that identity.

Your code can use a managed identity to request access tokens for services that support Microsoft Entra authentication. Azure takes care of rolling the credentials that are used by the service instance.

### Credential rotation

Credential rotation is controlled by the resource provider that hosts the Azure resource. The default rotation of the credential occurs every 46 days. It's up to the resource provider to call for new credentials, so the resource provider could wait longer than 46 days.

The following diagram shows how managed service identities work with Azure virtual machines (VMs):

:::image type="content" source="../media/az500-managed-identities-6f9fa72e.png" alt-text="Data flow described in the following text.":::


### How a system-assigned managed identity works with an Azure VM

1.  Azure Resource Manager receives a request to enable the system-assigned managed identity on a VM.
2.  Azure Resource Manager creates a service principal in Microsoft Entra ID for the identity of the VM. The service principal is created in the Microsoft Entra tenant that's trusted by the subscription.
3.  Azure Resource Manager configures the identity on the VM by updating the Azure Instance Metadata Service identity endpoint with the service principal client ID and certificate.
4.  After the VM has an identity, use the service principal information to grant the VM access to Azure resources. To call Azure Resource Manager, use role-based access control (RBAC) in Microsoft Entra ID to assign the appropriate role to the VM service principal. To call Key Vault, grant your code access to the specific secret or key in Key Vault.
5.  Your code that's running on the VM can request a token from the Azure Instance Metadata service endpoint, accessible only from within the VM: http://169.254.169.254/metadata/identity/oauth2/token
     -  The resource parameter specifies the service to which the token is sent. To authenticate to Azure Resource Manager, use resource=https://management.azure.com/.
     -  API version parameter specifies the IMDS version, use api-version=2018-02-01 or greater.
6.  A call is made to Microsoft Entra ID to request an access token (as specified in step 5) by using the client ID and certificate configured in step 3. Microsoft Entra ID returns a JSON Web Token (JWT) access token.
7.  Your code sends the access token on a call to a service that supports Microsoft Entra authentication
