# Palo Alto Networks VNet Module for Azure

A terraform module for deploying a Virtual Network and its components required for the VM-Series firewalls in Azure.

## Usage

```hcl
resource "azurerm_resource_group" "this" {
  name     = var.resource_group_name
  location = var.location
  tags     = {}
}

module "vnet" {
  source = "github.com/PaloAltoNetworks/terraform-azurerm-vmseries-modules//modules/vnet"

  virtual_network_name    = var.virtual_network_name
  resource_group_name     = azurerm_resource_group.this.name
  address_space           = var.address_space
  network_security_groups = var.network_security_groups
  route_tables            = var.route_tables
  subnets                 = var.subnets
  tags                    = var.tags

  depends_on = [azurerm_resource_group.this]
}
