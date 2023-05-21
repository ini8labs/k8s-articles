# k3s vs k8s

### k3s - Lightweight Kubernetes

- Lightweight kubernetes distribution, less than 100 MB.
- Meant to run small workloads.
- Can not be host in multi-cloud environment.

## Installing k3s

- Cluster mode
- Networking
- Database

- k3s can be installed in 2 modes:

    - Single Node mode: With one master node.
    - HA Mode: With Multiple master nodes.

### Installing k3s in single node mode

#### Manually

**Adding  a Master node**

- ssh into one of the nodes that would become master node:
- Run below command
  ```shell
  curl -sfL https://get.k3s.io | sh -
  ```
  
- Above command will install k3s along with kubectl, can run below command to validate the cluster installation
  ```shell
  root@anand-k8s-automate-3:~# kubectl get nodes
  NAME                   STATUS   ROLES                  AGE   VERSION
  anand-k8s-automate-3   Ready    control-plane,master   6s    v1.26.4+k3s1
  root@anand-k8s-automate-3:~#
  ```
    
**Joining a worker node**

- Get the node token from master node under the path ```/var/lib/rancher/k3s/server/node-token```
- ssh into the worker node.
- Create an env variable with the token value.
    ```shell
    export K3S_TOKEN=<token-value>
    ```

- Create another env variable with the k3s URL value, this is the IP address of master node with the port on which it
listens.
    ```shell
    export K3S_URL=https://<master-node-ip>:6443
    ```

- Run the below command to add the node as worker node in k3s cluster.
    ```shell
    curl -sfL https://get.k3s.io | K3S_URL=$K3S_URL K3S_TOKEN=$K3S_TOKEN sh -
    ```

- Login to master node to verify the status using `kubectl get nodes` command
    ```shell
    root@anand-k8s-automate-3:~# kubectl get nodes
    NAME                   STATUS   ROLES                  AGE   VERSION
    anand-k8s-automate-3   Ready    control-plane,master   15m   v1.26.4+k3s1
    anand-k8s-automate-2   Ready    <none>                 21s   v1.26.4+k3s1
    root@anand-k8s-automate-3:~#
  ```

#### Using Ansible For Automation



### Installing k3s in HA mode
