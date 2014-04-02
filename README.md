barbican-vagrant-zero
=====================

Vagrantfile/Berksfile to spin up a Barbican cluster with a single command (vagrant up).  The cluster includes an api node, a queue node, a worker node, and a database node.  The vagrantfile utlizes Chef Zero, and relies on Berkshelf to pull down all the necessary cookbooks.

Install the following plugins in exactly this order:

```
$ vagrant plugin install vagrant-omnibus

$ vagrant plugin install vagrant-chef-zero

$ vagrant plugin install vagrant-berkshelf
```

Then simply vagrant up!
