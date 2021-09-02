Deploy OpenSearch using Ansible
================================

## Single node OpenSearch Installation

This ansible playbook supports the following,

- Can be deployed on baremetal and VMs(AWS EC2)
- Supports most popular **Linux distributions**(Centos7, RHEL7)
- Install and configure the Apache2.0 opensource OpenSearch
- Configure TLS/SSL for OpenSearch transport layer(Nodes to Nodes communication) and REST API layer
- Generate self-signed certificates to configure TLS/SSL for opensearch
- Configure the Internal Users Database with limited users and user-defined passwords

Prerequisite
------------
- **Ansible**
- **Java 8**

Configure
---------

Refer the file `inventories/opensearch/group_vars/all/all.yml` to change the default values.

For example we need to increase the java memory heap size for opensearch,

    xms_value: 8
    xmx_value: 8

You can set the reserved users(`admin` and `kibanaserver`) password using `admin_password` and `kibanaserver_password` variables.

Install
-------

### Ansible

    # Deploy with ansible playbook - run the playbook as root
    ansible-playbook -i inventories/opensearch/hosts opensearch.yml

It will install and configure the opensearch. Once the deployment completed, you can access the opensearch with user `admin` and password which you provided for variable `admin_password`(Default password: Test@123).

## TBD
- opensearch multi-node cluster setup
- opensearch dashbaords Installation
- Performance analyzer plugin configuration
