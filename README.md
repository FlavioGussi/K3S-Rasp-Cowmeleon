
# Requisitos

* 2 Raspberry Pi's

## Configuração do Raspberry

Use o Raspberry Pi Imager ([RPI](https://www.raspberrypi.com/software/)) para fazer a instalação do S.O.

* Modifique o arquivo /boot/cmdline
```bash
echo "cgroup_memory=1 cgroup_enable=memory" >> /boot/cmdline
```


* Modifique o arquivo /boot/config
```bash
echo "arm_64bit=1" >> /boot/config
```



## Configuração do K3S

Configure IP Tables em modo Legacy

```bash
sudo iptables -F sudo update-alternatives --set iptables /usr/sbin/iptables-legacy 
sudo update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy 
sudo reboot
```

## Instalação K3S (Nó Master)
* Entre em modo Root

```bash
sudo su -
```

* Instale o K3S (Configuração do Master Node)

```bash
curl -sfL https://get.k3s.io | K3S_KUBECONFIG_MODE="644" sh -s -
```

* Após a instalação, pegue o Token do Node Master

```bash
sudo cat /var/lib/rancher/k3s/server/node-token
```
## Instalação K3S (Nó Minion)

* Execute o seguinte comando em todos os Nós que irão compor o Cluster

```bash
curl -sfL https://get.k3s.io | K3S_TOKEN="SEU_TOKEN" K3S_URL="https://[SEU_SERVIDOR]:6443" K3S_NODE_NAME="NOME_DO_NODE" sh -
```

* Para verificar se tudo esta correto, execute o seguinte comando

```bash
kubectl get nodes -o wide
```
 O resultado deve ser algo semelhante a isso

```bash
NAME     STATUS     ROLES                  AGE     VERSION        INTERNAL-IP     EXTERNAL-IP   OS-IMAGE                         KERNEL-VERSION   CONTAINER-RUNTIME
node1    NotReady   <none>                 25h     v1.21.5+k3s2   192.168.0.103   <none>        Raspbian GNU/Linux 10 (buster)   5.10.63-v8+      containerd://1.4.11-k3s1
node2    Ready      <none>                 2d15h   v1.21.5+k3s2   192.168.0.84    <none>        Raspbian GNU/Linux 10 (buster)   5.10.63-v7l+     containerd://1.4.11-k3s1
master   Ready      control-plane,master   38h     v1.21.5+k3s2   192.168.0.83    <none>        Raspbian GNU/Linux 10 (buster)   5.10.63-v8+      containerd://1.4.11-k3s1

```

Pronto, você já tem um cluster criado

# Links Úteis

* [K3S.io](https://k3s.io)
* [K3S Rancher](https://rancher.com/docs/k3s/latest/en/)
* [Github K3S](https://github.com/k3s-io/k3s)
