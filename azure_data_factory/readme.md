# Azure Data Factory Deployment for Exporting whed_org Table to CSV

This repository provides an Azure Resource Manager (ARM) template to deploy an Azure Data Factory (ADF) instance that copies data from a MySQL database table (whed_org) and exports it as a .csv file to Azure Blob Storage. The deployment includes configurations for enhanced security using private endpoints.

## Overview

This project automates the deployment of an Azure Data Factory instance configured to:

-   **Source**: Connect to a MySQL database and access the whed_org table.
-   **Destination**: Export the data to a CSV file stored in Azure Blob Storage.
-   **Security**: Utilize private endpoints to ensure secure communication between services.

## Architecture

-   **Azure Data Factory**: Orchestrates the data movement.
-   **MySQL Database**: Source of the whed_org table.
-   **Azure Blob Storage**: Destination for the CSV file.
-   **Private Endpoints**: Secure communication channels between services.

## Prerequisites

-   **Azure Subscription**: An active subscription with sufficient permissions.
-   **Azure CLI**: Installed and configured. [Install Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
-   **Resource Groups**: Created for your Storage Account and Data Factory.
-   **MySQL Database**: Accessible and contains the whed_org table.
-   **Storage Account**: Configured to accept private endpoint connections.

## Deployment Instructions

### 1. Create the Azure Data Factory Resource

Before deploying the ARM template, create the Azure Data Factory resource to avoid the "factory not available" error:

```bash
az datafactory create \
  --name "cosc320-data-factory" \
  --resource-group "YourDataFactoryResourceGroup" \
  --location "francecentral"
```

### 2. Clone the Repository

```bash
git clone https://github.com/yourusername/WHED_Cloud_Infra.git
cd your-repo-name
```

### 3. Update the Parameters File

Navigate to the `parameters.json` file associated with the Data Factory ARM template and update the following parameters:

-   `factoryName`: Name of your Data Factory.
-   `AzureMySql1_connectionString`: Your MySQL connection string.
-   `AzureBlobStorage1_connectionString`: Your Storage Account connection string (if not using Managed Identity).
-   `storageAccountResourceGroup`: Resource group containing your Storage Account.
-   `storageAccountName`: Name of your Storage Account.
-   `storageAccountFQDN`: FQDN of your Storage Account (e.g., yourstorageaccount.blob.core.windows.net).
-   `groupId`: Set to "blob" (default value).

### 4. Secure Connection Strings

**Important**: Do not store sensitive information like connection strings in source control. Consider the following options to secure your connection strings:

-   **Azure Key Vault**: Store secrets in Key Vault and reference them in your ARM template.
-   **Azure Managed Identity**: Use Managed Identity for Azure services to authenticate without secrets.
-   **Deployment-Time Parameters**: Pass connection strings as parameters during deployment without storing them in files.

### 5. Deploy the Data Factory Configuration

Deploy the Data Factory configuration using the updated `parameters.json` file:

```bash
az deployment group create \
  --resource-group YourDataFactoryResourceGroup \
  --template-file path/to/your/data_factory_template.json \
  --parameters path/to/your/data_factory_parameters.json
```

### 6. Configuring Private Endpoints

This deployment uses private endpoints to enhance security. Here's how to configure them for both blob storage and your database:

### Blob Storage Private Endpoint

The blob storage private endpoint is configured automatically through the ARM template deployment. After deployment:

1. Navigate to your Storage Account in the Azure Portal.
2. Go to Networking > Private endpoint connections.
3. You should see a pending private endpoint connection from your Data Factory.
4. Select the connection and click "Approve". Add a description if desired.
5. Verify that the connection state changes to "Approved".

### Database Private Endpoint

For the database, you need to configure the private endpoint manually in the Azure Data Factory UI:

1. Open your Azure Data Factory in the Azure Portal.
2. Click on "Author & Monitor" to open the ADF studio.
3. In the studio, go to the "Manage" tab.
4. Under "Managed Private Endpoints", click "New".
5. Select your Azure subscription and the MySQL database resource.
6. Give your private endpoint a name and click "Create".

After creation:

7. Go back to your MySQL database resource in the Azure Portal.
8. Navigate to Networking > Private endpoint connections.
9. Find the pending connection from ADF and approve it.

### Verifying Connections

After configuring both private endpoints:

1. In ADF studio, go to the "Manage" tab.
2. Check your linked services for both blob storage and MySQL.
3. Click "Test connection" on each to ensure they're working correctly through the private endpoints.

Note: It may take a few minutes for the private endpoint connections to become fully operational after approval.

## Usage

After deployment:

1. Access the Data Factory in the Azure Portal.
2. Open the Data Factory Studio and go to Author & Monitor.
3. Locate the pipeline named `cosc320_whed_org_export`.
4. Click Debug to test the pipeline or Add Trigger to schedule it.
5. Verify the output in your Azure Blob Storage account in the `cosc320-adf-export` container.

## Configuration Details

## Data Factory Components

### Linked Services

-   **AzureMySql1**: Connects to the MySQL database.
-   **AzureBlobStorage1**: Connects to the Azure Blob Storage.

### Datasets

-   **SourceDataset_48w**: References the `whed_org` table in the MySQL database.
-   **DestinationDataset_48w**: Defines the CSV file in Blob Storage.

### Pipeline

-   **cosc320_whed_org_export**: Contains the copy activity to move data from source to destination.

## Linked Services Details

### AzureMySql1

-   **Type**: `AzureMySql`
-   **Connection String**: Parameterized for security.

### AzureBlobStorage1

-   **Type**: `AzureBlobStorage`
-   **Connection String**: Parameterized or uses Managed Identity.

## Datasets Details

### SourceDataset_48w

-   **Type**: `AzureMySqlTable`
-   **Linked Service**: `AzureMySql1`
-   **Table Name**: `whed_org`

### DestinationDataset_48w

-   **Type**: `DelimitedText`
-   **Linked Service**: `AzureBlobStorage1`
-   **File Name**: `whed_org.csv`
-   **Container**: `cosc320-adf-export`

## Pipeline Details

### cosc320_whed_org_export

#### Activities

-   **Copy_48w**: Copies data from the MySQL table to the CSV file.

#### Configurations

-   **Source**: `SourceDataset_48w`
-   **Sink**: `DestinationDataset_48w`
-   **Mappings**: Auto-mapped based on the schema.

## Security Considerations

Connection Strings:

Store securely and avoid exposing them.
Use Azure Key Vault or Managed Identity where possible.

Private Endpoints:

Ensure private endpoint connections are approved.
Verify that network policies allow required traffic.

Firewall Rules:

Configure Storage Account firewall to deny public access if using private endpoints.
Allow specific IP ranges if necessary.

## Troubleshooting

Deployment Errors:

Ensure all parameters are correctly specified.
Verify that resource names are unique and comply with Azure naming conventions.

Private Endpoint Issues:

Confirm that private endpoint connections are approved.
Check for any pending approvals in the Storage Account.

Pipeline Failures:

Check activity logs in Data Factory for detailed error messages.
Ensure that the MySQL database is accessible and that credentials are correct.

## Notes

-   **Region Selection**: The templates default to deploying resources in the France Central region. Adjust as needed.
-   **Compatibility**: The ARM templates use specific API versions. Ensure compatibility with your Azure subscription.
-   **Data Export for Other Tables**: This ARM template only exports data for whed_org, additional tables can be exported by modifying the template file.

## Disclaimer

This template is intended for educational purposes and may not adhere to all security and performance best practices suitable for production environments. Before deploying in a production setting, review and adjust the configurations to meet the necessary compliance and security standards.
