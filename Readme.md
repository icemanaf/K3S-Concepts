note  append  to the first line in this  file sudo nano /boot/cmdline.txt for all pi's!
  cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory


install K3S master curl -sfL https://get.k3s.io | sh -

get the token sudo cat /var/lib/rancher/k3s/server/node-token

192.168.0.80 (k3s Master) k3s Token K102812b94390ea9bc2279e7739d18dad8f26dd295da9bbfb184b2029d51c5147b3::server:65d8c7416379988141bae208a42402b4

installing the agent.
1) add the host name

3) run curl -sfL http://get.k3s.io | K3S_URL=https://RPI80:6443 \
K3S_TOKEN=K102812b94390ea9bc2279e7739d18dad8f26dd295da9bbfb184b2029d51c5147b3::server:65d8c7416379988141bae208a42402b4 sh -
4) check agent status sudo systemctl status k3s-agent

5) check  /etc/systemd/system/k3s-agent.service.env file to change the K3S_URL to https://192.168.0.80:6443 and then restart the k3s-agent serv if the hostname RPI80
is not resolveable.


192.168.0.82/83 (influx and grafana)

192.168.0.84/85

=====
installing kubectl locally on a windows machine.
1) install chocolatey.
2)do the following on a admin enabled powershell console 
3)run choco install kubernetes-cli 
4)navigate to the home directory cd ~\
5)create a .kube directory mkdir .kube
6) copy over the \etc\rancher\k3s\k3s.yaml file from the master over to the kube directory and rename it as config.
7)kubectl get nodes command should give you the nodes of your remote clustert.

