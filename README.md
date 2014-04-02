barbican-vagrant-zero
=====================

Vagrantfile/Berksfile to spin up a Barbican cluster with a single command (vagrant up). The vagrantfile utilizes Chef Zero, and relies on Berkshelf to pull down all the necessary cookbooks. 

The cluster includes 4 virtual machines:
* API node
* Worker node 
* Queue node (RabbitMQ)
* Database node (Postgresql)  
 
Install the following plugins in exactly this order:

```
$ vagrant plugin install vagrant-omnibus

$ vagrant plugin install vagrant-chef-zero

$ vagrant plugin install vagrant-berkshelf
```

Then simply vagrant up!

Tested with versions up to vagrant 1.4.3 and berkshelf 2.0.14.
