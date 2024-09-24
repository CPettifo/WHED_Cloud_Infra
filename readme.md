# COSC320 Project

This project has two components:
1: Hosting of WHED MySQL database in Azure
2: Automating the deployment of an Azure infrastructure to export data to a CSV file in Azure Blob Storage using Azure Data Factory.

## Project Overview

The project consists of the following main components:

1. Virtual Network
2. Storage Account
3. Azure Data Factory
4. MySQL Database (can be created independently)

## Deployment Order

To successfully deploy this project, follow these steps in order:

### 1. Create Virtual Network

Set up the virtual network that will be used by other resources.

📘 [Read Virtual Network README](./azure_vnet/README.md)

### 2. Deploy Storage Account

Create the storage account that will store the exported CSV file.

📘 [Read Storage Account README](./azure_storage/README.md)

### 3. Set Up Azure Data Factory

Deploy and configure Azure Data Factory to orchestrate the data movement.

📘 [Read Azure Data Factory README](./azure_data_factory/README.md)

### 4. MySQL Database (Independent)

The MySQL database can be created independently of this workflow. Ensure it's accessible and contains the `whed_org` table before running the Data Factory pipeline.

## Additional Information

-   **Security**: Each component has specific security considerations. Refer to individual README files for details.
-   **Private Endpoints**: This project uses private endpoints for enhanced security. Configuration steps are provided in component-specific instructions.

## Prerequisites

-   Azure Subscription with necessary permissions
-   Azure CLI installed and configured
-   Basic knowledge of Azure services and ARM templates

## Post-Deployment

After deploying all components:

1. Verify all resources are created successfully.
2. Check private endpoint connections are approved and functional.
3. Test the Data Factory pipeline to ensure data is correctly exported from MySQL to Blob Storage.

## Maintenance

Regularly review and update your deployed resources to ensure they comply with the latest security best practices and your organizational requirements.

## Note

For detailed instructions on each component, please refer to the linked README files.
