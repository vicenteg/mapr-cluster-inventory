mapr-cluster-inventory
======================

This is an ansible dynamic inventory script for MapR Hadoop clusters.

The script connects to the MCS HTTP API and requests a node list. Using the configured service information, the script will automatically generate groups in the format ansible expects. You can use this script to dynamically create an inventory for your cluster at runtime. This enables things like this, against a cluster about which you have no prior knowledge:

```
ansible cldb -i mapr-cluster-inventory -m setup
```

If you'd like to inspect the system details of CLDB nodes.


Usage
=====

For use with ansible as a dynamic inventory script, you must create some environment variables. The below invocation of ansible also assumes that the inventory script is in the current working directory.

```
export MCS_NODE_LIST_URL="https://<webserver>:8443/rest/node/list"
export MCS_USERNAME=<mcs_username>
export MCS_PASSWORD=<mcs_password>

ansible all -i mapr-cluster-inventory -m ping
```

