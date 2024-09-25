# Azure Database for MySQL Flexible Server ARM Template

This repository contains an Azure Resource Manager (ARM) template for deploying an Azure Database for MySQL Flexible Server. This template is designed for a university project using an Azure account, with a focus on functionality and cost-efficiency.

## Template Overview

The ARM template deploys the following resources:

-   An Azure Database for MySQL Flexible Server in the France Central region
-   Multiple databases within the server
-   Firewall rules for the server
-   Server configurations

## Security Considerations

Given the educational nature of this project, certain security choices were made to balance functionality, learning, and cost:

1. **Firewall Rules**:

    - Option to allow all IP addresses or specify allowed IP ranges
    - Configurable through parameters for flexibility in a learning environment

2. **SSL Enforcement**:

    - Enabled by default for secure connections

3. **Admin Login**:

    - Configurable through parameters
    - It's recommended to use a strong, unique password

4. **Backup Retention**:

    - Configurable, with a default of 7 days
    - Balances data protection and storage costs

5. **Geo-Redundant Backup**:
    - Disabled by default to reduce costs
    - Can be enabled if needed for higher data protection

## Usage

To deploy this template:

1. Log in to your Azure account:

    ```
    az login
    ```

2. Create a resource group (if not already created):

    ```
    az group create --name YourResourceGroupName --location francecentral
    ```

3. Deploy the template:
    ```
    az deployment group create --resource-group YourResourceGroupName --template-file path/to/your/mysql_template.json --parameters path/to/your/mysql_parameters.json
    ```

## Parameters

-   `serverName`: Name of the MySQL Flexible Server
-   `administratorLogin`: Admin username for the server
-   `location`: Azure region for deployment
-   `skuName`: SKU name for the server
-   `skuTier`: SKU tier (e.g., Burstable, General Purpose)
-   `storageSize`: Storage size in GB
-   `backupRetentionDays`: Number of days to retain backups
-   `geoRedundantBackup`: Enable or disable geo-redundant backups
-   `databases`: Array of database names to create
-   `allowAllIPs`: Boolean to allow all IP addresses
-   `allowedIpAddresses`: Array of allowed IP address ranges

## Notes

-   This template is designed for educational purposes and may not meet all security best practices for production environments.
-   The API version used is 2023-12-30.
-   Review and adjust the server configuration as needed for your specific project requirements.

## Cost Considerations

-   Geo-redundant backup is disabled by default to reduce costs.
-   The France Central region is used, but you may change this to a region that offers better pricing or is closer to your location.

## Disclaimer

This template is for educational purposes only. For production use, additional security measures and optimizations should be implemented.
