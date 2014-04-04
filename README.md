Setting up a Barbican Cluster using Vagrant
===========================================

This details the use of [Vagrant](http://docs.vagrantup.com/) to stand up a minimal Barbican cluster with a few commands. The **Vagrantfile** utilizes Chef Zero, and relies on Berkshelf to pull down all the necessary cookbooks.

The cluster will consist of four virtual machines:
* API node
* Worker node 
* Queue node (RabbitMQ)
* Database node (Postgresql)

**Note:** We are assuming you already have [Vagrant](http://docs.vagrantup.com/v2/getting-started/index.html) 1.4.3 installed and working. Newer versions of Vagrant are not currently compatible with the plugins we will be using. You will want to ensure you have a computer with enough RAM to run four virtual machines, 8GB of RAM should be enough but you can probably get away with 4GB of RAM.

### Plugin Installation
 
Install the following plugins in this order:

```
$ vagrant plugin install vagrant-omnibus
$ vagrant plugin install vagrant-berkshelf
$ vagrant plugin install vagrant-chef-zero
```

### Clone Repo and Start the Cluster

With the plugins successfully installed you need to clone this repository and then **vagrant up**:

```
$ git clone https://github.com/cloudkeep-ops/barbican-vagrant-zero.git
$ cd barbican-vagrant-zero
$ vagrant up
```

### Accessing the Nodes in the Cluster

Wait for the virtual machines to fully load. At that point you can access each machine via SSH:

```
$ vagrant ssh [db|queue|api|worker]
```

**Example, SSH into the worker node:**

```
$ vagrant ssh worker
```

The nodes on the cluster will be assigned the following IPv4 addresses:


Node Name     | IPv4 Address
------------- | -------------
Queue         | 192.168.2.2
DB            | 192.168.2.3
API           | 192.168.2.4
Worker        | 192.168.2.5

Your host machine will be assigned **192.168.2.1**

