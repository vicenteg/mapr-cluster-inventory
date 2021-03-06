mapr-cluster-inventory
======================

This is an [Ansible](http://www.ansible.com) dynamic inventory script for [MapR Hadoop](http://www.mapr.com) clusters.

The script connects to the MCS HTTP API and requests a node list. Using the configured service information, the script will automatically generate groups in the format ansible expects. You can use this script to dynamically create an [ansible inventory](http://docs.ansible.com/intro_inventory.html) for your cluster at runtime. 

Dynamic inventory allows you to do stuff like this:

```
ansible cldb -i mapr-cluster-inventory -m setup
```

If you'd like to inspect the system details of CLDB nodes.

Or perhaps you'd like to push your (previously generated) SSH public key to the entire cluster:

```
ansible all -i mapr-cluster-inventory -m authorized_key -a "user=root key='`cat ~/.ssh/id_rsa.pub`'"
```

Note that ansible will, by default, attempt to connect to the remote machine using the same username as the currently logged in user. If you want to change that, invoke ansible with -u <user> as follows (taking the CLDB example from above):

```
ansible cldb -i mapr-cluster-inventory -m setup -u ubuntu
```


Setup
=====

The script requires two python modules, `requests` and `argparse` that may not be installed by default. 

For RHEL and CentOS, it's recommended to install the EPEL repository and install these modules with yum:

`yum -y install python-requests python-argparse`

For Ubuntu, you will need to figure out how to get those packages, as I have so far only tested with CentOS.


Usage
=====

For use with ansible as a dynamic inventory script, you must create some environment variables. The below invocation of ansible also assumes that the inventory script is in the current working directory.

```
export MCS_NODE_LIST_URL="https://<webserver>:8443/rest/node/list"
export MCS_USERNAME=<mcs_username>
export MCS_PASSWORD=<mcs_password>

ansible all -i mapr-cluster-inventory -m ping
```

Note: For a large cluster with hosts you trust, you may want to [disable host key checking](http://docs.ansible.com/intro_configuration.html#host-key-checking)  to avoid being prompted over and over if this is the first time you've connected to the hosts via SSH.
