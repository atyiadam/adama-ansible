# Directory structure

This repository follows the [Sample directory layout](https://docs.ansible.com/ansible/latest/tips_tricks/sample_setup.html#sample-directory-layout) from Ansible.

# Processes

## RKE2 cluster provisioning

1. First provision the servers & agents via Terraform
2. Add new hosts to inventory
3. Generic (non-k8s) server config:
```
$ ansible-playbook -i prod playbooks/infrastructure/site.yml --limit <INVENTORY_CLUSTER_NAME>
```
4. RKE2 bootstrap with keepalived
```
$ ansible-playbook -i prod playbooks/operations/rke2/rke2-bootstrap-cluster.yml \
-e cluster_name="k8s_01" \
-e cluster_env="prod" \
-e cluster_vip="10.10.30.147" \
-e kubernetes_api_fqdn="kubernetes-api.k8s-01.prod.home.adamatyi.com" \
-e rke2_token="your-secure-token" 
```
