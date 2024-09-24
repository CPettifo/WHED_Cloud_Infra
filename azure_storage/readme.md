# Azure Storage Account ARM Template

This repository contains an Azure Resource Manager (ARM) template for deploying a Storage Account with associated configurations. This template is designed for a university project using a free Azure account, focusing on functionality, learning, and cost-efficiency.

## Template Overview

The ARM template deploys the following resources:

### Azure Storage Account:

-   Type: StorageV2 (general-purpose v2)
-   Location: France Central region
-   Replication: Standard Locally Redundant Storage (LRS)
-   Hierarchical namespace enabled (for Azure Data Lake Storage Gen2)
-   Large file shares enabled

### Blob Services:

-   Default blob service configuration
-   Container delete retention policy enabled (7 days)
-   Delete retention policy enabled (7 days)

### File Services:

-   Default file service configuration
-   Share delete retention policy enabled (7 days)

### Queue and Table Services:

-   Default configurations with CORS rules set to empty

### Containers:

-   `cosc320-adf-export`
-   `cosc320-audit-logs-db`
-   `insights-logs-mysqlauditlogs`

Each container has:

-   Default encryption scope
-   Public access set to None

### Network Configuration:

-   Virtual network rules to restrict access to a specified VNet and subnet
-   IP address rules to allow access from a specific IP address
-   Default action set to Deny to enhance security

## Security Considerations

Given the constraints of a free account and the educational nature of this project, certain security choices were made to balance functionality, learning, and cost:

-   **Encryption**: Encryption at rest is enabled using Microsoft-managed keys. Immutable storage with versioning is disabled for simplicity. Default encryption scope is used.
-   **Network Access Control**: Access is restricted to a specific virtual network and subnet. An additional IP addresses are allowed for access when configured from settings. Default action is set to Deny to prevent unauthorized access.

## Prerequisites

-   **Azure Account**: Ensure you have an active Azure subscription. A free account is sufficient for this project.
-   **Azure CLI**: Installed and logged in. You can download it [here](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

## Usage

### Step 1: Deploy the Network ARM Template

Before deploying the Storage Account, you need to deploy the Virtual Network (VNet) ARM template, as the Storage Account depends on the VNet for network access control.

Deploy the VNet Template:

```bash
az deployment group create \
  --resource-group YourResourceGroupName \
  --template-file path/to/your/vnet_template.json \
  --parameters path/to/your/vnet_parameters.json
```

### Step 2: Deploy the Storage Account ARM Template

1. Modify Parameters:

Update the `parameters.json` file with your specific values:

-   `storageAccounts_cosc320storage_name`: Name of the Storage Account.
-   `vnetResourceGroup`: Name of the resource group containing your VNet.
-   `vnetName`: Name of your Virtual Network.
-   `subnetName`: Name of the subnet within your VNet.
-   `allowedIpAddress`: Your public IP address to allow access.

2. Deploy the Template:

```bash
az deployment group create \
  --resource-group YourResourceGroupName \
  --template-file path/to/your/storage_template.json \
  --parameters path/to/your/storage_parameters.json
```

## Parameters

-   **storageAccounts_cosc320storage_name** (String):

    -   Description: Name of the Storage Account to create.
    -   Example: "myuniquestorageacct"

-   **vnetResourceGroup** (String):

    -   Description: Name of the resource group containing the Virtual Network.
    -   Example: "MyResourceGroup"

-   **vnetName** (String):

    -   Description: Name of the Virtual Network.
    -   Example: "my-vnet"

-   **subnetName** (String):

    -   Description: Name of the subnet within the Virtual Network.
    -   Example: "my-subnet"

-   **allowedIpAddress** (String):
    -   Description: The public IP address to allow access to the Storage Account.
    -   Example: "123.45.67.89"

## Notes

-   **Region**: The Storage Account is deployed in the France Central region. You may change this to a region closer to you or based on your requirements.
-   **API Version**: The template uses API version 2023-05-01.
-   **Resource Naming**: Adjust resource names to avoid conflicts, especially if deploying in a shared or limited namespace.

## Cost Considerations

-   **Free Account Limitations**:
    -   This template is designed to work within the constraints of a free Azure account.
    -   Features that incur additional costs, like advanced threat protection, are not enabled.
-   **Storage Account Type**:
    -   Uses Standard LRS to minimize costs.
    -   Hierarchical namespace and large file shares are enabled for educational purposes.

## Important Notes

-   **Run Network Template First**: The Storage Account depends on the VNet and subnet specified. Ensure that the network resources are deployed before running this Storage Account template.
-   **Security Best Practices**:
    -   This template is for educational purposes and may not meet all security best practices for production environments.
    -   In a production environment, consider enabling advanced security features and stricter access controls.
-   **Parameter Validation**:
    -   Ensure that the parameter values provided correspond to existing resources, especially for the VNet and subnet.

## Disclaimer

This template is intended for educational purposes only. It may not adhere to all security and performance best practices suitable for production environments. Before deploying in a production setting, review and adjust the configurations to meet the necessary compliance and security standards.
