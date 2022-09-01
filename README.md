<img src="https://opensearch.org/assets/brand/SVG/Logo/opensearch_logo_default.svg" height="64px"/>

- [OpenSearch Project Ansible-Playbook](#helm-charts)
- [OpenSearch Installation with Dashboards](opensearch-installation-with-dashboards)
- [Contributing](#contributing)
- [Getting Help](#getting-help)
- [Code of Conduct](#code-of-conduct)
- [Security](#security)
- [License](#license)
- [Copyright](#copyright)

## OpenSearch Project Ansible-Playbook

A community repository for Ansible Playbook of OpenSearch Project.

## Version and Branching
As of now, this ansible-playbook repository maintains 2 branches:
* _main_ (Version is 2.x.x for both `os_version` and `os_dashboards_version` in `inventories/opensearch/group_vars/all/all.yml`)
* _1.x_ (Version is 1.x.x for both `os_version` and `os_dashboards_version` in `inventories/opensearch/group_vars/all/all.yml`)
<br>

Contributors should choose the corresponding branch(es) when commiting their change(s):
* If you have a change for a specific version, only open PR to specific branch
* If you have a change for all available versions, first open a PR on `main`, then open a backport PR with `[backport 1.x]` in the title, with label `backport 1.x`, etc.

## OpenSearch Installation with Dashboards

This ansible playbook supports the following,

- Can be deployed on baremetal and VMs(AWS EC2)
- Supports most popular **Linux distributions**(Centos7, RHEL7, Amazon Linux2, Ubuntu 20.04)
- Install and configure the Apache2.0 opensource OpenSearch
- Configure TLS/SSL for OpenSearch transport layer(Nodes to Nodes communication) and REST API layer
- Generate self-signed certificates to configure TLS/SSL for opensearch
- Configure the Internal Users Database with limited users and user-defined passwords
- Configuration of authentication and authorization via OpenID
- Overriding default settings with your own
- Install and configure the Apache2.0 opensource OpenSearch Dashboards

### Prerequisite

- **Ansible 2.9+**
- **Java 8**

### Configure

Refer the file `inventories/opensearch/group_vars/all/all.yml` to change the default values.

For example if we need to increase the java memory heap size for opensearch,

    xms_value: 8
    xmx_value: 8

In `inventories/opensearch/hosts` file, you can configure the node details.
`ansible_host` is used for ansible to connect the nodes to run this playbook.
`ip` is used in OpenSearch and Dashboards configuration.

In AWS EC2,
```
os1 ansible_host=<Elastic/Public IP> address ansible_user=root ip=<Private IP address>
```

#### Multi-node Installation

By default, this playbook will install five nodes opensearch cluster with respective roles (3 master, 5 data and 2 ingest nodes).

```
os1 ansible_host=10.0.1.1 ip=10.0.1.1 roles=data,master
os2 ansible_host=10.0.1.2 ip=10.0.1.2 roles=data,master
os3 ansible_host=10.0.1.3 ip=10.0.1.3 roles=data,master
os4 ansible_host=10.0.1.4 ip=10.0.1.4 roles=data,ingest
os5 ansible_host=10.0.1.5 ip=10.0.1.5 roles=data,ingest
```

**Note**: You need to add additional nodes details in `inventories/opensearch/hosts` file for creating opensearch cluster with different node sizes.

For example, if you want to create seven nodes cluster with two additional data nodes (`os6` and `os7`) then you need to include the below entries.

```
os6 ansible_host=10.0.1.6 ip=10.0.1.6 roles=data
os7 ansible_host=10.0.1.7 ip=10.0.1.5 roles=data
```

You have to mention the opensearch node [roles](https://opensearch.org/docs/latest/opensearch/cluster/) details in `roles` variable.

#### Single Node Installation

For single node installation, you need to change the `cluster_type` variable in inventory file `inventories/opensearch/group_vars/all/all.yml`

```
cluster_type: single-node
```

### Install


    # Deploy with ansible playbook - run the playbook as root
    ansible-playbook -i inventories/opensearch/hosts opensearch.yml --extra-vars "admin_password=Test@123 kibanaserver_password=Test@6789"

You should set the reserved users(`admin` and `kibanaserver`) password using `admin_password` and `kibanaserver_password` variables.

If you define your own internal users (in addition to the reserved `admin` and `kibanaserver`) in custom configuration 
files, then passwords to them should be set via variables on the principle of `<username>_password`

It will install and configure the opensearch. Once the deployment completed, you can access the opensearch Dashboards with user `admin` and password which you provided for variable `admin_password`.

    # Deploy with ansible playbook - run the playbook as non-root user which have sudo privileges,
    ansible-playbook -i inventories/opensearch/hosts opensearch.yml --extra-vars "admin_password=Test@123 kibanaserver_password=Test@6789" --become

**Note**: Change the user details in `ansible_user` parameter in `inventories/opensearch/hosts`  inventory file.

### OpenID authentification
To enable authentication via OpenID, you need to change the `auth_type` variable in the inventory file 
`inventories/opensearch/group_vars/all/all.yml` by setting the value `oidc` and prescribe the necessary settings 
in the `oidc:` block.

### Custom configuration files

To override the default settings files, you need to put your settings in the `files` directory. The files should be 
named exactly the same as the original ones (internal_users.yml, roles.yml, tenants.yml, etc.)

Especially note the file `files/internal_users.yml`. If it exists and the `copy_custom_security_configs: true` setting is enabled, 
then only in this case the task of setting passwords for internal users from variables is started. If the file `internal_users.yml` 
is not located in the `files` directory, but, for example, in one of its subdirectories, then playbook will not work correctly

### IaC (Infrastructure-as-Code)

If you want to use the role not only for the initial deployment of the cluster, but also for further management of it, 
then set the `iac_enable` parameter to `true`. 

By default, if the /tmp/opensearch-nodecerts directory with certificates exists on the server from which the playbook 
is launched, it is assumed that the configuration has not changed and some settings are not copied to the target servers.

Conversely, if the /tmp/opensearch-nodecerts directory does not exist on the server from which the playbook is launched, 
then new certificates and settings are generated and they are copied to the target servers.

If you use this repository not only for the initial deployment of the cluster, but also for its automatic configuration 
via CI/CD, then new certificates will be generated every time the pipeline is launched, overwriting existing ones, which 
is not always necessary if the cluster is already in production.

When iac_enable enabling, and all the cluster servers have all the necessary certificates, they will not be copied again. 
If at least on one server (for example, when adding a new server to the cluster) if there is not at least one certificate 
from the list, then all certificates on all cluster servers will be updated

Also, if the option is enabled, the settings files will be updated with each execution (previously, the settings were 
updated only if the /tmp/opensearch-nodecerts directory was missing on the server from which the playbook was launched 
and new certificates were generated)

## Contributing

See [developer guide](DEVELOPER_GUIDE.md) and [how to contribute to this project](CONTRIBUTING.md).

## Getting Help

If you find a bug, or have a feature request, please don't hesitate to open an issue in this repository.

For more information, see [project website](https://opensearch.org/) and [documentation](https://docs-beta.opensearch.org/). If you need help and are unsure where to open an issue, try [forums](https://discuss.opendistrocommunity.dev/).

## Code of Conduct

This project has adopted the [Amazon Open Source Code of Conduct](CODE_OF_CONDUCT.md). For more information see the [Code of Conduct FAQ](https://aws.github.io/code-of-conduct-faq), or contact [opensource-codeofconduct@amazon.com](mailto:opensource-codeofconduct@amazon.com) with any additional questions or comments.

## Security

If you discover a potential security issue in this project we ask that you notify AWS/Amazon Security via our [vulnerability reporting page](http://aws.amazon.com/security/vulnerability-reporting/). Please do **not** create a public GitHub issue.

## License

This project is licensed under the [Apache v2.0 License](LICENSE.txt).

## Copyright

Copyright OpenSearch Contributors. See [NOTICE](NOTICE.txt) for details.
