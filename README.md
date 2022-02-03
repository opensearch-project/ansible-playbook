<img src="https://opensearch.org/assets/brand/SVG/Logo/opensearch_logo_default.svg" height="64px"/>

- [OpenSearch Project Ansible-Playbook](#helm-charts)
- [Single node OpenSearch Installation with Dashboards](single-node-opensearch-installation-with-dashboards)
- [Contributing](#contributing)
- [Getting Help](#getting-help)
- [Code of Conduct](#code-of-conduct)
- [Security](#security)
- [License](#license)
- [Copyright](#copyright)

## OpenSearch Project Ansible-Playbook

A community repository for Ansible Playbook of OpenSearch Project.

Fork from [http://github.com/opensearch-project/ansible-playbook] including
cluster initialization

## Single node OpenSearch Installation with Dashboards

This ansible playbook supports the following,

- Can be deployed on baremetal and VMs(AWS EC2)
- Supports most popular **Linux distributions**(Centos7, RHEL7, Debian 11)
- Install and configure the Apache2.0 opensource OpenSearch
- Configure TLS/SSL for OpenSearch transport layer(Nodes to Nodes communication) and REST API layer
- Generate self-signed certificates to configure TLS/SSL for opensearch
- Configure the Internal Users Database with limited users and user-defined passwords
- Install and configure the Apache2.0 opensource OpenSearch Dashboards

### Prerequisite

- **Ansible 2.9+**

### Configure & install 

See `example/README.md`

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
