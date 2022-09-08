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
