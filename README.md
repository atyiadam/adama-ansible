# Directory structure

This repository follows the [Sample directory layout](https://docs.ansible.com/ansible/latest/tips_tricks/sample_setup.html#sample-directory-layout) from Ansible.

# Processes

## RKE2 cluster provisioning

1. First provision the servers & agents via Terraform
2. Add new hosts to inventory
3. Add new cluster to `/playbooks/infrastructure/k8s/k8s-01-networking-configuration.yml`
4. Generic (non-k8s) server config:
```
$ ansible-playbook -i <ENV> playbooks/infrastructure/site.yml --limit <INVENTORY_CLUSTER_NAME>
```
5. RKE2 bootstrap with keepalived
```
$ ansible-playbook -i <CLUSTER_ENV> playbooks/operations/rke2/rke2-configure-cluster.yml \
-e cluster_name="<CLUSTER_NAME>" \
-e cluster_env="<CLUSTER_ENV>" \
-e cluster_vip="<CLUSTER_VIP>" \
-e kubernetes_api_fqdn="<K8S_API_FQDN>" \
-e rke2_token="<RKE2_TOKEN>" 
```
6. Create DNS A record for argocd
