## k3s - Ansible setup

By default this role will install a single node cluster on the provided server. The resulting cluster will also have Traefik 
and CoreDNS preinstalled.


### Multi Node
When writing this, k3s does not support multiple masters. It does however support multiple workers. To setup  a cluster with 
multiple workers you will need to do the following:

* Run the role on the master server. You will have to provide the following variables:
  - `k3s_master`: Needs to be set to `true`.
  - `k3s_cluster_secret`: A random secret that will be used to authenticate the nodes between each other.
* Then the role on the worker server. You will have to provide the following variables:
  - `k3s_master`: Needs to be set to `false`.
  - `k3s_cluster_secret`: The same secret as before.
  - `k3s_master_ip`: The IP of the master node.


### Cluster Access
Once the cluster is installed, you can find the kubeconfig file on the master node at `/etc/rancher/k3s/k3s.yaml`. To access the cluster, copy this file locally and update the `server: https://127.0.0.1:6443` line, changing the IP address to the IP address of the master node.

Then you can use this file to make calls on the cluster, e.g.:
```
kubectl --kubeconfig k3s.yaml get namespaces
```
**WARNING**: The kubeconfig file `k3s.yaml` contains admin-level credentials to control all kubernetes objects on the cluster. Handle with care!
