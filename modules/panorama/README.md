# Palo Alto Networks Panorama Module for Azure

A terraform module for deploying a working Panorama instance in Azure.

## Usage

```hcl
module "panorama" {
  source  = "PaloAltoNetworks/vmseries-modules/azurerm//modules/panorama"
  version = "0.1.0"

  panorama_name       = var.panorama_name
  resource_group_name = azurerm_resource_group.this.name
  location            = var.location
  avzone              = var.avzone // Optional Availability Zone number

  interface = [ // Only one interface in Panorama VM is supported
    {
      name               = "mgmt"
      subnet_id          = var.subnet_id
      public_ip          = true
      public_ip_name     = "panorama"
    }
  ]

  panorama_size               = var.panorama_size
  username                    = var.username
  password                    = random_password.this.result
  panorama_sku                = var.panorama_sku
  panorama_version            = var.panorama_version
  boot_diagnostic_storage_uri = module.bootstrap.storage_account.primary_blob_endpoint
  tags                        = var.tags
}
