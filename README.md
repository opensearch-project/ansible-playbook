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

## OpenSearch Installation with Dashboards

This ansible playbook supports the following,

- Can be deployed on baremetal and VMs(AWS EC2)
- Supports most popular **Linux distributions**(Centos7, RHEL7)
- Install and configure the Apache2.0 opensource OpenSearch
- Configure TLS/SSL for OpenSearch transport layer(Nodes to Nodes communication) and REST API layer
- Generate self-signed certificates to configure TLS/SSL for opensearch
- Configure the Internal Users Database with limited users and user-defined passwords
- Install and configure the Apache2.0 opensource OpenSearch Dashboards

### Prerequisite

- **Ansible**
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
0s4 ansible_host=10.0.1.4 ip=10.0.1.4 roles=data,ingest
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

It will install and configure the opensearch. Once the deployment completed, you can access the opensearch Dashboards with user `admin` and password which you provided for variable `admin_password`.

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
