# Setting up a K3S cluster on Raspberry Pi's.
### Notes
* you will need a few Raspberry Pi's. It is recommended to have one dedicated Pi for the master node and the rest can be setup as worker nodes.
### Setting up the PI's
* Setup the Pi's with the current version of Raspbian.Configure individual hostnames in each. Presumably they would be setup to run headless with ssh enabled, and the gpu memory set to the minimum recommended level. More details on how to do it can be found by searching the net.
* Enable cgroups.  Append  to the first line in this  file  /boot/cmdline.txt in all the pi's.
```
    cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory
```
* Install the K3S master by running the following command on the Pi selected as the master:
```
install K3S master curl -sfL https://get.k3s.io | sh -
```
* Retrieve the K3S Token by running the following on the master node , this is required to join the worker nodes with the master.
```
    sudo cat /var/lib/rancher/k3s/server/node-token
```
* Run the worker install's in each of the worker Pi's.
```
run curl -sfL http://get.k3s.io | K3S_URL=https://<Master Node Hostname or IP>:6443 K3S_TOKEN=<Token retrieved from above step> sh -
```
* Check if everything is up by running kubectl on the master node.
```
    kubectl get nodes
```
* if there are issues with the workers connecting to the master,check  /etc/systemd/system/k3s-agent.service.env file to change the K3S_URL to https://<IP address of master>:6443 and then restart the k3s-agent service if the master hostname is not resolveable.
* You may get tired of having to ssh into the master node to work on your cluster. One option is to install kubectl locally. Here are the instructions to do an installation on Windows. These worked on my Windows 10 machine.
    1) Install chocolatey.
    2) Do the following on a admin enabled powershell console 
    3) Run choco install kubernetes-cli 
    4) Navigate to the home directory cd ~\
    5) Create a .kube directory.
    6) Copy over the \etc\rancher\k3s\k3s.yaml file from the master over to the .kube directory and rename it as config.
    7) Test ``` kubectl get nodes ``` command should give you the nodes of your remote cluster.
