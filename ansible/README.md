Deploy a Production Ready Open Distro Elasticsearch Cluster and Kibana using Standalone Plugin Installation method
===================================================================================================================

This ansible playbook supports the following,

- Can be deployed on baremetal and VMs(AWS EC2)
- Supports most popular **Linux distributions**(Centos7, RHEL7)
- Install and configure the Apache2.0 opensource elasticsearch and kibana
- Install and configure the below Elasticsearch opendistro plugins using [standalone install method](https://opendistro.github.io/for-elasticsearch-docs/docs/install/plugins/)
    - Security
    - Anomaly Detection
    - Job Scheduler
    - Alerting
    - SQL
    - Index State Management
    - Asynchronous Search
    - Reports scheduler
- Install and configure the below Kibana opendistro plugins using [standalone install method](https://opendistro.github.io/for-elasticsearch-docs/docs/kibana/plugins/)
    - Security
    - Anomaly Detection
    - Alerting
    - Index State Management
    - Reports
    - SQL Workbench
- Configure TLS/SSL for elasticsearch transport layer(Nodes to Nodes communication) and REST API layer
- Generate self-signed certificates to configure TLS/SSL for elasticsearch
- Configure the Internal Users Database with limited users and user-defined passwords

Requirements
------------
- **Ansible**
- **Java 8**

Configure
---------

Refer the file `inventories/opendistro/group_vars/all/all.yml` to change the default values for elasticsearch and kibana Installation.

For example we need to increase the java memory heap size for elasticsearch,

    xms_value: 8
    xmx_value: 8

By default, it will install five nodes elasticsearch cluster with respective roles (3 master, 5 data and 2 ingest nodes).

```
es1 ansible_host=10.0.1.1 ip=10.0.1.1 roles=data,master
es2 ansible_host=10.0.1.2 ip=10.0.1.2 roles=data,master
es3 ansible_host=10.0.1.3 ip=10.0.1.3 roles=data,master
es4 ansible_host=10.0.1.4 ip=10.0.1.4 roles=data,ingest
es5 ansible_host=10.0.1.5 ip=10.0.1.5 roles=data,ingest
```

**Note**: You need to add additional nodes details in `inventories/opendistro/hosts` file for creating elasticsearch cluster with different node sizes.

For example, if you want to create seven nodes cluster with two additional data nodes (`es6` and `es7`) then you need to add the below entries.

```
es6 ansible_host=10.0.1.6 ip=10.0.1.6 roles=data
es7 ansible_host=10.0.1.7 ip=10.0.1.5 roles=data
```

You can mention the elasticsearch node roles details in `roles` parameter.

You can also change the elasticsearch and kibana versions. Please refer the version compatibility with [Open Distro](https://opendistro.github.io/for-elasticsearch-docs/version-history/).

Refer the file `inventories/opendistro/group_vars/all/opendistro.yml` to change the default values for Open Distro installation.

You can customize the opendistro plugins installation by enable or disable it in config file `opendistro.yml`

    opendistro_security_install: false
    opendistro_security_install_kibana: false

Note: By default, all the above mentioned plugins will be installed.

You can set the reserved users(`admin` and `kibanaserver`) password using `admin_password` and `kibanaserver_password` variables.

Install
-------

### Ansible

    # Deploy with ansible playbook - run the playbook as root
    ansible-playbook -i inventories/opendistro/hosts opendistro.yml

It will install and configure the elasticsearch and kibana with opendistro.
Once the deployment completed, you can access the kibana with user `admin` and password which you provided for variable `admin_password`(Default password: Test@123).
