# How to add a new image?
## cirros
* How to add Cirros Cloud image to Glance.
```
$ wget http://download.cirros-cloud.net/0.5.2/cirros-0.5.2-x86_64-disk.img  

$ openstack image create \
-container-format bare \
-disk-format qcow2 \
-file cirros-0.5.2-x86_64-disk.img   \
cirros
```
** Default login username for instances created from this image: cirros
Password: gocubsgo
## ubuntu
```
wget https://cloud-images.ubuntu.com/bionic/current/bionic-server-cloudimg-amd64.img


openstack image create \
    --container-format bare \
    --disk-format qcow2 \
    --file bionic-server-cloudimg-amd64.img \
    Ubuntu-18.04
```
```
#cloud-config

users:
  - name: pennandw
    groups: sudo
    passswd:$1$SaltSalt$YhgRYajLPrYevs14poKBQ0
    sudo: ALL=(ALL) NOPASSWD:ALL
    ssh-authorized-keys:
      - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC0H7BTmtCqvnnYcxp5X2xtIQ+KGvRMcxyMg8C+XDiV0oVXq7+FpPSup0yM0dl0dY7FgrVpzRrIg9hts7w32aA56vn+/B6sVHrv0ZqxXYlqA/UenWoTgST0bxauh5WqDjXvMnlPtF0PRC8jDc7vh0E/ID5v8SnTiaOypAIc9Tcxc4pQSAY+1cQv+z7NqN+1sHVVDe0WrbqZMTok1bk1zVHNzO9EDsoih6rrMUUMCFXNTT0XmrCWxKu50V13DrVuIp6GLoTcVsGuH8cZlcYcsEsAqcf1TTwkadVtfZxP2ekjSsHdb0r/ojSN9XJP1RyOV1t3T9XI6QtlafmYqFjNX453YTjAJHpmCgnDro+KTE0ptFrTiL7ZG9Om+ul0f1axHeeot/PxpRndfB9xSI02aN7rJvNE1XLbBtL9SIzkUAd+CgPXp1CpuuZKCL7n5wiJnR9o4bLj9jNM1NS8DdPDSHv9noyvwUTMflFSoJL+QBRvQIkukJ7v5uMeZgUNFvCZZC/QbuuEz2IgJ5EmQ1N1g19vDaguzBxuPBw+sabe+PThgTxUxiq7uFt/mi/1sQGhuvlRmvvvs+xnzk5h2ZGeHh8q56vDrM0wCTE8QbcAIxgGLQPte1LDkcxbxg/ynSiJglYgS1sc7Nqjqy/PlHxBJNbZ1SNL0+5rs288Yq8keCG4XQ== MY-KEY
    shell: /bin/bash
    
package_upgrade: true

packages:
  - git
  - wget
  - nginix
  - bash-completion
  
power_state:
  mode: reboot

```
```
passwd: $1$SaltSalt$YhgRYajLPrYevs14poKBQ0

The above password is generated for plain-text password "secret"

root@localBoxISO [ ~ ]# openssl passwd -1 -salt SaltSalt secret
$1$SaltSalt$YhgRYajLPrYevs14poKBQ0
root@localBoxISO [ ~ ]#

# mkpasswd --method=SHA-512 --rounds=4096
```
```
$ ssh cirros@192.168.56.252 -i my-key
```

```
$ openstack flavor create --public PA-VM-50 --ram 6144 --disk 0 --vcpus 2 --rxtx-factor 1

```
```
$ openstack image create --file /tmp/PA-VM-KVM-8.1.17.qcow2 --disk-format qcow2 --container-format bare --public  --unprotected --property hw_vif_multiqueue_enabled=true PAvFW_8_1_17_MQ
```
```
$ openstack availability zone list
+-----------+-------------+
| Zone Name | Zone Status |
+-----------+-------------+
| internal  | available   |
| nova      | available   |
| nova      | available   |
+-----------+-------------+

```
```
$ openstack security group create allow_ssh_https_icmp_secgroup
$ openstack security group rule create --proto tcp --remote-ip 0.0.0.0/0 --dst-port 22 allow_ssh_https_icmp_secgroup
$ openstack security group rule create --proto tcp --remote-ip 0.0.0.0/0 --dst-port 443 allow_ssh_https_icmp_secgroup
$ openstack security group rule create --proto icmp --remote-ip 0.0.0.0/0 allow_ssh_https_icmp_secgroup

```

```
$ openstack security group show allow_ssh_https_icmp_secgroup2

```
```
$ ssh-keygen -b 4096 -C MY-KEY -t rsa -N '' -f my-key
```
