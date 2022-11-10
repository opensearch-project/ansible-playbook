<img src="https://opensearch.org/assets/brand/SVG/Logo/opensearch_logo_default.svg" height="64px"/>

I have added following features to original [OpenSearch Ansible playbook](https://github.com/opensearch-project/ansible-playbook)
- Vargantfile for full stack testing
- fixed root-privilegies escalation on roles to able work not only under ansible root user
- "# vim:ft=ansible:"
- host_download: ability to download artifacts on ansible controller. remotes can in closed segment w/o inet. Also it can help with slow internet connection  

Enjoy the DevOps life!
