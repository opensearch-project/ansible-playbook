# Opensearch and Opensearch Dashboards example deployment

Tested on:

- Debian 11
- CentOS7

Only tested in cluster mode 3+ nodes - no single node extensive testing atm.
Also only deployed using self generated dummy PKI (not with externally provided
certifcates - i.e. from an external trusted or "enterprise" PKI)

## Inventory file

Set the right hostgroup name and the fqdns of the backend hosts to connect to,
you'll obviously need root or sudo/become access.

## Group vars

Adapt the group vars name (filename) to the name of the group in your inventory
file and group vars content if you need to.

## Playbook

Sample playbook in `opensearch-cluster.yml`:

The example playbook will first install the OpenSearch cluster (3 nodes) then
deploy Opensearch Dashboards on first node of the cluster - adapt it to your
needs.

The cluster configuration is located in the `group_vars/` YAML file

The Opensearch Dashboards role configuration should be changed for these
parameters:

- adapt `hosts:` of the dashboards system for you inventory (second part of
  playbook) - it can be a dedicated host
- adapt hostgroup name to match inventory and group vars file
  (`opensearch_dashboards_backend_hostgroup`)
- adapt kibana server backend password to match the one in group vars
  (`opensearch_dashboards_backend_password`)
- adapt `opensearch_cluster_name` to match the cluster name in the group vars
- 
