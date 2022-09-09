# TERRAFORM-001
## Description:
This lab is to create a basic build 
using terraform
# CODE
```
terraform init
```
```
terraform plan
```
```
teraform apply
```

## Build a simple server
```
$ cat providers.tf
# Define required providers
terraform {
required_version = ">= 0.14.0"
  required_providers {
    openstack = {
      source  = "terraform-provider-openstack/openstack"
      version = "~> 1.35.0"
    }
  }
}

# Configure the OpenStack Provider
provider "openstack" {
  user_name   = "admin"
  tenant_name = "admin"
  password    = "redhat"
  auth_url    = "http://192.168.56.103/identity"
  region      = "RegionOne"
}

# Using data sources

data "openstack_compute_flavor_v2" "flavor_1" {
 name = "m1.tiny"
  }

data "openstack_images_image_v2" "cirros" {
 name = "cirros-0.5.2-x86_64-disk"
  }

data "openstack_networking_network_v2" "network_1"{
 name = "shared"
}

# Creating resoucres

resource "openstack_networking_floatingip_v2" "floatip_1" {
  pool = "public"
}
resource "openstack_networking_port_v2" "port_1" {
  network_id = "${data.openstack_networking_network_v2.network_1.id}"
}

resource "openstack_compute_floatingip_associate_v2" "fip_1" {
  floating_ip = "${openstack_networking_floatingip_v2.floatip_1.address}"
  instance_id = "${openstack_compute_instance_v2.my_instance.id}"
}


# Create a test server
resource "openstack_compute_instance_v2" "my_instance" {
  name      = "my_instance"
  image_name  = "cirros-0.5.2-x86_64-disk"
  flavor_name = "m1.tiny"
  key_pair  = "my-key"
  security_groups = ["default", "OPEN"]
  network {
    name = "shared"
  }

}
```
