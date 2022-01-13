# Load Balancer Module for Azure

A Terraform module for deploying a Load Balancer for VM-Series firewalls. Supports both standalone and scale set deployments. Supports either inbound or outbound configuration.

The module creates a single load balancer and a single backend for it, but it allows multiple frontends.

## Usage

```hcl
# Deploy the inbound load balancer for traffic into the azure environment
module "inbound_lb" {
  source = "github.com/PaloAltoNetworks/terraform-azurerm-vmseries-modules//modules/loadbalancer"

  resource_group_name = ""
  location            = ""
  name                = ""
  probe_name          = ""
  backend_name        = ""
  frontend_ips = {
    # Map of maps (each object has one frontend to many backend relationship) 
    pip-existing = {
      create_public_ip     = false
      public_ip_address_id = ""
      rules = {
        HTTP = {
          port         = 80
          protocol     = "Tcp"
        }
      }
    }
  }
}

# Deploy the outbound load balancer for traffic into the azure environment
module "outbound_lb" {
  source = "github.com/PaloAltoNetworks/terraform-azurerm-vmseries-modules//modules/loadbalancer"
  
  resource_group_name = ""
  location            = ""
  name                = ""
  probe_name          = ""
  backend_name        = ""
  frontend_ips = {
    internal_fe = {
      subnet_id                     = ""
      private_ip_address_allocation = "Static" // Dynamic or Static
      private_ip_address            = "10.0.1.6" 
      rules = {
        HA_PORTS = {
          port         = 0
          protocol     = "All"
        }
      }
    }
  }
}
