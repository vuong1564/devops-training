### Chuẩn bị môi trường

### allow root ssh and change passs

```bash

sudo sed -i 's/^#* *\(PermitRootLogin\)\(.*\)$/\1 yes/' /etc/ssh/sshd_config
sudo sed -i 's/^#* *\(PasswordAuthentication\)\(.*\)$/\1 yes/' /etc/ssh/sshd_config
sudo systemctl restart sshd.service
echo -e "admin@123\nadmin@123" | (sudo passwd vagrant)
echo -e "admin@123\nadmin@123" | (sudo passwd root)

```

### install docker all node
```bash

sudo apt-get update
sudo apt-get install -y \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo service docker start
sudo systemctl enable docker
sudo service docker status

sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker

```
```bash

sudo vi /etc/docker/daemon.json

```
```bash
{
  "insecure-registries" : ["nexus.digital.vn"]
}
```
```bash
sudo systemctl daemon-reload
sudo systemctl restart docker

echo -e "jenkins\n123123aA@" | (docker login nexus.digital.vn)

echo "net.bridge.bridge-nf-call-ip6tables=1" | sudo tee -a /etc/sysctl.conf
echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

### Thực hiện pull image từ docker
```bash
docker pull rancher/mirrored-coreos-etcd:v3.5.4
docker pull rancher/rke-tools:v0.1.87
docker pull rancher/mirrored-k8s-dns-kube-dns:1.21.1
docker pull rancher/mirrored-k8s-dns-dnsmasq-nanny:1.21.1
docker pull rancher/mirrored-k8s-dns-sidecar:1.21.1
docker pull rancher/mirrored-cluster-proportional-autoscaler:1.8.5
docker pull rancher/mirrored-coredns-coredns:1.9.3
docker pull rancher/mirrored-k8s-dns-node-cache:1.21.1
docker pull rancher/hyperkube:v1.24.4-rancher1
docker pull rancher/mirrored-coreos-flannel:v0.15.1
docker pull rancher/flannel-cni:v0.3.0-rancher6
docker pull rancher/mirrored-calico-node:v3.22.0
docker pull rancher/calico-cni:v3.22.0-rancher1
docker pull rancher/mirrored-calico-kube-controllers:v3.22.0
docker pull rancher/mirrored-calico-ctl:v3.22.0
docker pull rancher/mirrored-calico-pod2daemon-flexvol:v3.22.0
docker pull rancher/mirrored-flannelcni-flannel:v0.17.0
docker pull weaveworks/weave-kube:2.8.1
docker pull weaveworks/weave-npc:2.8.1
docker pull rancher/mirrored-pause:3.6
docker pull rancher/nginx-ingress-controller:nginx-1.2.1-rancher1
docker pull rancher/mirrored-nginx-ingress-controller-defaultbackend:1.5-rancher1
docker pull rancher/mirrored-ingress-nginx-kube-webhook-certgen:v1.1.1
docker pull rancher/mirrored-metrics-server:v0.6.1
docker pull noiro/cnideploy:5.2.3.2.1d150da
docker pull noiro/aci-containers-host:5.2.3.2.1d150da
docker pull noiro/opflex:5.2.3.2.1d150da
docker pull noiro/openvswitch:5.2.3.2.1d150da
docker pull noiro/aci-containers-controller:5.2.3.2.1d150da
docker pull noiro/gbp-server:5.2.3.2.1d150da
docker pull noiro/opflex-server:5.2.3.2.1d150da
```

### Thực hiện export image 

```bash
docker save rancher/mirrored-coreos-etcd:v3.5.4 | gzip > mirrored-coreos-etcd.tar.gz
docker save rancher/rke-tools:v0.1.87 | gzip > rke-tools.tar.gz
docker save rancher/mirrored-k8s-dns-kube-dns:1.21.1 | gzip > mirrored-k8s-dns-kube-dns.tar.gz
docker save rancher/mirrored-k8s-dns-dnsmasq-nanny:1.21.1 | gzip > mirrored-k8s-dns-dnsmasq-nanny.tar.gz
docker save rancher/mirrored-k8s-dns-sidecar:1.21.1 | gzip > mirrored-k8s-dns-sidecar.tar.gz
docker save rancher/mirrored-cluster-proportional-autoscaler:1.8.5 | gzip > mirrored-cluster-proportional-autoscaler.tar.gz
docker save rancher/mirrored-coredns-coredns:1.9.3 | gzip > mirrored-coredns-coredns.tar.gz
docker save rancher/mirrored-k8s-dns-node-cache:1.21.1 | gzip > mirrored-k8s-dns-node-cache.tar.gz
docker save rancher/hyperkube:v1.24.4-rancher1 | gzip > hyperkube.tar.gz
docker save rancher/mirrored-coreos-flannel:v0.15.1 | gzip > mirrored-coreos-flannel.tar.gz
docker save rancher/flannel-cni:v0.3.0-rancher6 | gzip > flannel-cni.tar.gz
docker save rancher/mirrored-calico-node:v3.22.0 | gzip > mirrored-calico-node.tar.gz
docker save rancher/calico-cni:v3.22.0-rancher1 | gzip > calico-cni.tar.gz
docker save rancher/mirrored-calico-kube-controllers:v3.22.0 | gzip > mirrored-calico-kube-controllers.tar.gz
docker save rancher/mirrored-calico-ctl:v3.22.0 | gzip > mirrored-calico-ctl.tar.gz
docker save rancher/mirrored-calico-pod2daemon-flexvol:v3.22.0 | gzip > mirrored-calico-pod2daemon-flexvol.tar.gz
docker save rancher/mirrored-flannelcni-flannel:v0.17.0 | gzip > mirrored-flannelcni-flannel.tar.gz
docker save weaveworks/weave-kube:2.8.1 | gzip > weave-kube.tar.gz
docker save weaveworks/weave-npc:2.8.1 | gzip > weave-npc.tar.gz
docker save rancher/mirrored-pause:3.6 | gzip > mirrored-pause.tar.gz
docker save rancher/nginx-ingress-controller:nginx-1.2.1-rancher1 | gzip > nginx-ingress-controller.tar.gz
docker save rancher/mirrored-nginx-ingress-controller-defaultbackend:1.5-rancher1 | gzip > mirrored-nginx-ingress-controller-defaultbackend.tar.gz
docker save rancher/mirrored-ingress-nginx-kube-webhook-certgen:v1.1.1 | gzip > mirrored-ingress-nginx-kube-webhook-certgen.tar.gz
docker save rancher/mirrored-metrics-server:v0.6.1 | gzip > rancher/mirrored-metrics-server.tar.gz
docker save noiro/cnideploy:5.2.3.2.1d150da | gzip > cnideploy.tar.gz
docker save noiro/aci-containers-host:5.2.3.2.1d150da | gzip > aci-containers-host.tar.gz
docker save noiro/opflex:5.2.3.2.1d150da | gzip > opflex.tar.gz
docker save noiro/openvswitch:5.2.3.2.1d150da | gzip > openvswitch.tar.gz
docker save noiro/aci-containers-controller:5.2.3.2.1d150da | gzip > aci-containers-controller.tar.gz
docker save noiro/gbp-server:5.2.3.2.1d150da | gzip > gbp-server.tar.gz
docker save noiro/opflex-server:5.2.3.2.1d150da | gzip > opflex-server.tar.gz
```

### Thực hiện import image 

```bash
cat mirrored-coreos-etcd.tar.gz |docker import - rancher/mirrored-coreos-etcd:v3.5.4 
cat rke-tools.tar.gz |docker import - rancher/rke-tools:v0.1.87 
cat mirrored-k8s-dns-kube-dns.tar.gz |docker import - rancher/mirrored-k8s-dns-kube-dns:1.21.1 
cat mirrored-k8s-dns-dnsmasq-nanny.tar.gz docker import - rancher/mirrored-k8s-dns-dnsmasq-nanny:1.21.1 |
cat mirrored-k8s-dns-sidecar.tar.gz |docker import - rancher/mirrored-k8s-dns-sidecar:1.21.1 
cat mirrored-cluster-proportional-autoscaler.tar.gz |docker import - rancher/mirrored-cluster-proportional-autoscaler:1.8.5 
cat mirrored-coredns-coredns.tar.gz |docker import - rancher/mirrored-coredns-coredns:1.9.3 
cat mirrored-k8s-dns-node-cache.tar.gz |docker import - rancher/mirrored-k8s-dns-node-cache:1.21.1 
cat hyperkube.tar.gz |docker import - rancher/hyperkube:v1.24.4-rancher1 
cat mirrored-coreos-flannel.tar.gz |docker import - rancher/mirrored-coreos-flannel:v0.15.1 
cat flannel-cni.tar.gz |docker import - rancher/flannel-cni:v0.3.0-rancher6 
cat mirrored-calico-node.tar.gz |docker import - rancher/mirrored-calico-node:v3.22.0 
cat calico-cni.tar.gz |docker import - rancher/calico-cni:v3.22.0-rancher1 
cat mirrored-calico-kube-controllers.tar.gz |docker import - rancher/mirrored-calico-kube-controllers:v3.22.0 
cat mirrored-calico-ctl.tar.gz |docker import - rancher/mirrored-calico-ctl:v3.22.0 
cat mirrored-calico-pod2daemon-flexvol.tar.gz |docker import - rancher/mirrored-calico-pod2daemon-flexvol:v3.22.0 
cat mirrored-flannelcni-flannel.tar.gz |docker import - rancher/mirrored-flannelcni-flannel:v0.17.0 
cat weave-kube.tar.gz |docker import - weaveworks/weave-kube:2.8.1 
cat weave-npc.tar.gz |docker import - weaveworks/weave-npc:2.8.1 
cat mirrored-pause.tar.gz |docker import - rancher/mirrored-pause:3.6 
cat nginx-ingress-controller.tar.gz |docker import - rancher/nginx-ingress-controller:nginx-1.2.1-rancher1 
cat mirrored-nginx-ingress-controller-defaultbackend.tar.gz |docker import - rancher/mirrored-nginx-ingress-controller-defaultbackend:1.5-rancher1 
cat mirrored-ingress-nginx-kube-webhook-certgen.tar.gz |docker import - rancher/mirrored-ingress-nginx-kube-webhook-certgen:v1.1.1 
cat rancher/mirrored-metrics-server.tar.gz |docker import - rancher/mirrored-metrics-server:v0.6.1 
cat cnideploy.tar.gz |docker import - noiro/cnideploy:5.2.3.2.1d150da 
cat aci-containers-host.tar.gz |docker import - noiro/aci-containers-host:5.2.3.2.1d150da 
cat opflex.tar.gz |docker import - noiro/opflex:5.2.3.2.1d150da 
cat openvswitch.tar.gz |docker import - noiro/openvswitch:5.2.3.2.1d150da 
cat aci-containers-controller.tar.gz |docker import - noiro/aci-containers-controller:5.2.3.2.1d150da 
cat gbp-server.tar.gz |docker import - noiro/gbp-server:5.2.3.2.1d150da 
cat opflex-server.tar.gz |docker import - noiro/opflex-server:5.2.3.2.1d150da 

```

### Thực hiện sửa tag image 


```bash

docker image tag rancher/mirrored-coreos-etcd:v3.5.4 vuong1564/mirrored-coreos-etcd:v3.5.4
docker image tag rancher/rke-tools:v0.1.87 vuong1564/rke-tools:v0.1.87
docker image tag rancher/mirrored-k8s-dns-kube-dns:1.21.1 vuong1564/mirrored-k8s-dns-kube-dns:1.21.1
docker image tag rancher/mirrored-k8s-dns-dnsmasq-nanny:1.21.1 vuong1564/mirrored-k8s-dns-dnsmasq-nanny:1.21.1
docker image tag rancher/mirrored-k8s-dns-sidecar:1.21.1 vuong1564/mirrored-k8s-dns-sidecar:1.21.1
docker image tag rancher/mirrored-cluster-proportional-autoscaler:1.8.5 vuong1564/mirrored-cluster-proportional-autoscaler:1.8.5
docker image tag rancher/mirrored-coredns-coredns:1.9.3 vuong1564/mirrored-coredns-coredns:1.9.3 
docker image tag rancher/mirrored-k8s-dns-node-cache:1.21.1 vuong1564/mirrored-k8s-dns-node-cache:1.21.1
docker image tag rancher/hyperkube:v1.24.4-rancher1 vuong1564/hyperkube:v1.24.4-rancher1
docker image tag rancher/mirrored-coreos-flannel:v0.15.1 vuong1564/mirrored-coreos-flannel:v0.15.1
docker image tag rancher/flannel-cni:v0.3.0-rancher6 vuong1564/flannel-cni:v0.3.0-rancher6
docker image tag rancher/mirrored-calico-node:v3.22.0 vuong1564/mirrored-calico-node:v3.22.0
docker image tag rancher/calico-cni:v3.22.0-rancher1 vuong1564/calico-cni:v3.22.0-rancher1
docker image tag rancher/mirrored-calico-kube-controllers:v3.22.0 vuong1564/mirrored-calico-kube-controllers:v3.22.0
docker image tag rancher/mirrored-calico-ctl:v3.22.0 vuong1564/mirrored-calico-ctl:v3.22.0
docker image tag rancher/mirrored-calico-pod2daemon-flexvol:v3.22.0 vuong1564/mirrored-calico-pod2daemon-flexvol:v3.22.0
docker image tag rancher/mirrored-flannelcni-flannel:v0.17.0 image tag vuong1564/mirrored-flannelcni-flannel:v0.17.0
docker image tag weaveworks/weave-kube:2.8.1 vuong1564/weave-kube:2.8.1 
docker image tag weaveworks/weave-npc:2.8.1 vuong1564/weave-npc:2.8.1 
docker image tag rancher/mirrored-pause:3.6 vuong1564/mirrored-pause:3.6
docker image tag rancher/nginx-ingress-controller:nginx-1.2.1-rancher1 vuong1564/nginx-ingress-controller:nginx-1.2.1-rancher1
docker image tag rancher/mirrored-nginx-ingress-controller-defaultbackend:1.5-rancher1 vuong1564/mirrored-nginx-ingress-controller-defaultbackend:1.5-rancher1
docker image tag rancher/mirrored-ingress-nginx-kube-webhook-certgen:v1.1.1 vuong1564/mirrored-ingress-nginx-kube-webhook-certgen:v1.1.1 
docker image tag rancher/mirrored-metrics-server:v0.6.1 vuong1564/mirrored-metrics-server:v0.6.1 
docker image tag noiro/cnideploy:5.2.3.2.1d150da vuong1564/cnideploy:5.2.3.2.1d150da
docker image tag noiro/aci-containers-host:5.2.3.2.1d150da vuong1564/aci-containers-host:5.2.3.2.1d150da
docker image tag noiro/opflex:5.2.3.2.1d150da vuong1564/opflex:5.2.3.2.1d150da 
docker image tag noiro/openvswitch:5.2.3.2.1d150da vuong1564/openvswitch:5.2.3.2.1d150da
docker image tag noiro/aci-containers-controller:5.2.3.2.1d150da vuong1564/aci-containers-controller:5.2.3.2.1d150da
docker image tag noiro/gbp-server:5.2.3.2.1d150da vuong1564/gbp-server:5.2.3.2.1d150da 
docker image tag noiro/opflex-server:5.2.3.2.1d150da vuong1564/opflex-server:5.2.3.2.1d150da 

```

### Thực hiện push image 

```bash

docker push vuong1564/mirrored-coreos-etcd:v3.5.4
docker push vuong1564/rke-tools:v0.1.87
docker push vuong1564/mirrored-k8s-dns-kube-dns:1.21.1
docker push vuong1564/mirrored-k8s-dns-dnsmasq-nanny:1.21.1
docker push vuong1564/mirrored-k8s-dns-sidecar:1.21.1
docker push vuong1564/mirrored-cluster-proportional-autoscaler:1.8.5
docker push vuong1564/mirrored-coredns-coredns:1.9.3
docker push vuong1564/mirrored-k8s-dns-node-cache:1.21.1
docker push vuong1564/hyperkube:v1.24.4-rancher1
docker push vuong1564/mirrored-coreos-flannel:v0.15.1
docker push vuong1564/flannel-cni:v0.3.0-rancher6
docker push vuong1564/mirrored-calico-node:v3.22.0
docker push vuong1564/calico-cni:v3.22.0-rancher1
docker push vuong1564/mirrored-calico-kube-controllers:v3.22.0
docker push vuong1564/mirrored-calico-ctl:v3.22.0
docker push vuong1564/mirrored-calico-pod2daemon-flexvol:v3.22.0
docker push vuong1564/mirrored-flannelcni-flannel:v0.17.0
docker push vuong1564/weave-kube:2.8.1
docker push vuong1564/weave-npc:2.8.1
docker push vuong1564/mirrored-pause:3.6
docker push vuong1564/nginx-ingress-controller:nginx-1.2.1-rancher1
docker push vuong1564/mirrored-nginx-ingress-controller-defaultbackend:1.5-rancher1
docker push vuong1564/mirrored-ingress-nginx-kube-webhook-certgen:v1.1.1
docker push vuong1564/mirrored-metrics-server:v0.6.1
docker push vuong1564/cnideploy:5.2.3.2.1d150da
docker push vuong1564/aci-containers-host:5.2.3.2.1d150da
docker push vuong1564/opflex:5.2.3.2.1d150da
docker push vuong1564/openvswitch:5.2.3.2.1d150da
docker push vuong1564/aci-containers-controller:5.2.3.2.1d150da
docker push vuong1564/gbp-server:5.2.3.2.1d150da
docker push vuong1564/opflex-server:5.2.3.2.1d150da

```


### rke bootrap 

Thực hiện gen ssh key cho host

```bash

ssh-keygen

```
Thực hiện copy key tới các host cần cài đặt

```bash

ssh-copy-id <user>@<host>

```

Thực hiện gen config của rke

```bash

rke config

```

Thực hiện bootrap cụm cluster

```bash

rke up --config cluster.yml

```

### remove all config rke

```bash
docker rm -f $(docker ps -qa)
docker rmi -f $(docker images -q)
docker volume rm $(docker volume ls -q)
for mount in $(mount | grep tmpfs | grep '/var/lib/kubelet' | awk '{ print $3 }') /var/lib/kubelet /var/lib/rancher; do umount $mount; done

rm -rf /etc/ceph \
       /etc/cni \
       /etc/kubernetes \
       /opt/cni \
       /opt/rke \
       /run/secrets/kubernetes.io \
       /run/calico \
       /run/flannel \
       /var/lib/calico \
       /var/lib/etcd \
       /var/lib/cni \
       /var/lib/kubelet \
       /var/lib/rancher/rke/log \
       /var/log/containers \
       /var/log/kube-audit \
       /var/log/pods \
       /var/run/calico
init 6

```

### install Jenkins
```bash
vi Dockerfile
```
```bash
FROM jenkins/jenkins:latest-jdk8
USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
 signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
 https://download.docker.com/linux/debian \
 $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins
```
```bash
docker build -t vuong1564/vuongnd-jenkins:v1.2 .
docker network create jenkins

docker container run --name my-jenkins-v2 -d --network jenkins -p 8080:8080 --privileged -p 5000:5000 \
-v /var/run/docker.sock:/var/run/docker.sock -v jenkins_home_v2:/var/jenkins_home --group-add $(stat \
-c '%g' /var/run/docker.sock) vuong1564/vuongnd-jenkins:v1.2
```
