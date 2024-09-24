# Azure Virtual Network ARM Template

This repository contains an Azure Resource Manager (ARM) template for deploying a Virtual Network (VNet) with a subnet. This template is designed for a university project using a free Azure account, with a focus on functionality and cost-efficiency.

## Template Overview

The ARM template deploys the following resources:

-   A Virtual Network (VNet) in the France Central region
-   A subnet within the VNet
-   Service endpoints for Azure Storage

## Security Considerations

Given the constraints of a free account and the educational nature of this project, certain security choices were made to balance functionality, learning, and cost:

1. **Encryption**:

    - Enabled for the VNet
    - Set to "AllowUnencrypted" to provide flexibility in a learning environment

2. **DDoS Protection**:

    - Disabled to avoid additional costs
    - Note: In a production environment, enabling DDoS protection is recommended

3. **Private Endpoint Policies**:

    - Disabled on the subnet to allow for future private endpoint configurations

4. **Address Space**:
    - Uses a broad range (10.0.0.0/16 for VNet, 10.0.0.0/24 for subnet)
    - Provides ample space for learning and experimentation

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
    az deployment group create --resource-group YourResourceGroupName --template-file path/to/your/vnet_template.json
    ```

## Parameters

-   `virtualNetworks_cosc320_vnet_name`: Name of the Virtual Network (default: "cosc320-vnet")

## Notes

-   This template is designed for educational purposes and may not meet all security best practices for production environments.
-   The API version used is 2024-01-01.
-   Review and adjust the network configuration as needed for your specific project requirements.

## Cost Considerations

-   This template is designed to work within the constraints of a free Azure account.
-   DDoS protection is disabled to avoid additional costs.
-   The France Central region is used, but you may change this to a region that offers better pricing or is closer to your location.

## Disclaimer

This template is for educational purposes only. For production use, additional security measures and optimizations should be implemented.
