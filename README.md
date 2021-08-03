# Multi Node - Multi GPU training in Dora [PyTorch]

This repo is adapted from the amazing work of https://github.com/yangkky
following the guide at.

https://yangkky.github.io/2019/07/08/distributed-pytorch-tutorial.html

## Run on Dora via CLI


```yaml
---
apiVersion: v1
kind: Workload
metadata:
  zone: dc-rov-01
  group: amedeo.setti
  name: violet.girl
spec:
  replica:
    count: 4
  driver: Docker
  selectors:
    gpu:
      product_name: All
      count: 2
  image: 
    image: paolorota/mixamo:dgx_cvpr2021
    pullPolicy: IfNotPresent
  config:
    cmd: /bin/bash
  volumes:
    - name: home
      target: /home
  network:
    mode: host # This is important
    ports: []
```

Once created, open 4 terminals, one for each container replica.
**If your home is under network storage, you can type
these command only on one container.**

```sh
cd /home
git clone https://github.com/adda25/dora-pytorch-multinode-docker
cd dora-pytorch-multinode-docker
```

Prepare the dataset:

```sh
wget www.di.ens.fr/~lelarge/MNIST.tar.gz
tar -zxvf MNIST.tar.gz	
```

Then, launch the simulation, for every container.

**Change the ip address and the interface based on your hardware**

On the master node:

```sh
./run_double.sh 4 2 0 192.168.180.80 8888 enp2s0
```

On the client nodes:

```sh
./run_double.sh 4 2 1 192.168.180.80 8888 enp2s0
```

```sh
./run_double.sh 4 2 2 192.168.180.80 8888 enp2s0
```

```sh
./run_double.sh 4 2 3 192.168.180.80 8888 enp2s0
```

Arguments of *run_double.sh* are:

- total number of nodes
- gpu count per node
- rank
- master address
- master port
- interface

