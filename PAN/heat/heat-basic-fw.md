# BASIC FIREWALL DEPLOYMENT USING HEAT TEMPLATE

**Prerequisites:**
- [ ] Untrust Network is created and available
- [ ] Trust Network is created and available
- [ ] HA Network is created and available
- [ ] OAM Network is created and available

**Diagram:**

# CODE
```
heat_template_version: 2015-04-30

description: Example template to deploy a PAN firewall

parameters:
  pan_image:
    type: string
    label: PAN image name
    description: PAN virtual firewall image to be used for firewall instance
    default: pa-vm-7.1.4

  pan_flavor:
    type: string
    label: Flavor
    description: Type of instance (flavor) to be used for PAN firewall instance
    default: m1.medium

  mgmt_network:
    type: string
    label: Management network name
    description: Network to attach management interface of PAN firewall instance to.

  public_network:
    type: string
    label: Public network name
    description: Public network for which floating IP addresses will be allocated

resources:
  pan_untrust_port:
    type: OS::Neutron::Port
    properties:
      network: shared
      security_groups:
        - allow_ssh_https_icmp_secgroup 
      fixed_ips:
        - subnet: shared-subnet
          ip_address: "192.168.233.10"

  pan_untrust_floating_ip:
    type: OS::Neutron::FloatingIP
    properties:
      floating_network: { get_param: public_network }

  pan_untrust_floating_ip_assoc:
    type: OS::Neutron::FloatingIPAssociation
    properties:
      floatingip_id: { get_resource: pan_untrust_floating_ip }
      port_id: { get_resource: pan_untrust_port }

  pan_trust_port:
    type: OS::Neutron::Port
    properties:
      network: trusted
      security_groups:
        - allow_ssh_https_icmp_secgroup
      fixed_ips:
        - subnet: trusted

  pan_fw_instance:
    type: OS::Nova::Server
    properties:
      image: { get_param: pan_image }
      flavor: { get_param: pan_flavor }
      networks:
        - network: { get_param: mgmt_network }
        - port: { get_resource: pan_untrust_port }
        - port: { get_resource: pan_trust_port }
      user_data_format: RAW
      config_drive: true
      user_data:
         {get_file: "./pan.tgz.base64"}

outputs:
  pan_fw_name:
    description: Name of the PAN firewall
    value: { get_attr: [pan_fw_instance, name] }
  pan_fw_mgmt_ip:
    description: Management IP address of the PAN firewall in mgmt network
    value: { get_attr: [pan_fw_instance, first_address] }
  pan_fw_public_ip:
    description: Floating IP address of PAN firewall in public network
    value: { get_attr: [ pan_untrust_floating_ip, floating_ip_address ] }
```
## COMMANDS
* Show all compute instances:
```
openstack server list
```
