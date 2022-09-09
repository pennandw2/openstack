# CODE
```
soon come
```

Description:   Use ansible and heat to deploy a webserver.





***I'm going to install ansible on the STAX box (OPENSTACK server) to run the simple play book.
ubuntu$ sudo apt install ansible ansible-core -y



HEAT TEMPLATE:
heat_template_version: 2013-05-23
description: Launch a basic instance with CirrOS image using the
 ``m1.tiny`` flavor, ``mykey`` key, and one network.
parameters:
					
  key_name:
    type: string
    description: Name of a KeyPair to enable SSH access to instance
  instance_type:
    type: string
    description: Provides the size, or flavor, of the VM server
    default: m1.tiny``
  image_id:
    type: string
    description: >
      name of the ID of the image to use for the server
    default: cirros-0.5.2-x86_64-disk
  network_name:
    type: string
    description: The network to use


resources:
  server:
    type: OS::Nova::Server
    properties:
      networks:
        - network: { get_param: network_name }
      image: { get_param: image_name }
      flavor: { get_param: flavor_name }
      key_name: { get_param: key_name }

        #outputs:
        #instance_name:
        # description: Name of the instance.
        # value: { get_attr: [ server, name ] }
        # instance_ip:
        # description: IP address of the instance.
        # value: { get_attr: [ server, first_address ] }


  $ openstack stack create -t heat_cirros.yaml  --parameter key_name=heat_key --parameter network_name=public  second_stack



ANSIBLE PLAYBOOK:
 
---
- name: stack
  hosts: localhost
  tasks:
  - name: Create Stack Server
    os_stack:
      name: "Server"
      state: present
      template: "../heat_templates/heat_server_instance.yaml"
      parameters:
          instance_name: ServerVM
          image_name: m1.small
          keypair_name: heat_key
          security_group: Demo
          network_name: private
          floating_ip_pool: public
![image](https://user-images.githubusercontent.com/69496058/189376064-fa964882-ba20-4eee-9d00-d56e3e73acd8.png)
