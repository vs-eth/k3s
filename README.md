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
