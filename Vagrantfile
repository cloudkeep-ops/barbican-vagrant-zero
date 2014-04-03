# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.require_plugin "vagrant-chef-zero"
Vagrant.require_plugin "vagrant-berkshelf"

Vagrant.configure("2") do |config|

  # Berkshelf plugin configuration
  config.berkshelf.enabled = true

  # Chef-Zero plugin configuration
  config.chef_zero.enabled = true

  # Omnibus plugin configuration
  config.omnibus.chef_version = :latest

  chef_environment = "vagrant"

  config.vm.define :queue do |queue|
    queue.vm.box = "centos-6.5"
    queue.vm.box_url = "https://ca9f9b62ae1b1e47c4f8-989e703834df24142dff402333777a7c.ssl.cf1.rackcdn.com/centos-6.5-provisionerless.box"
    queue.vm.network :private_network, ip: "192.168.2.2"
    queue.vm.hostname = 'queue'
    queue.vm.provision :chef_client do |chef|
      chef.json = {
        :vagrant => {
          :ip => "192.168.2.2"
        },
        :barbican_rabbitmq => {
          :discovery => {
            :search_query => 'node_group_tag:queue'
          }
        }
      }
      chef.run_list = [
          "recipe[barbican-rabbitmq]"
      ]
    end

  end

  config.vm.define :db do |db|
    db.vm.box = "centos-6.5"
    db.vm.box_url = "https://ca9f9b62ae1b1e47c4f8-989e703834df24142dff402333777a7c.ssl.cf1.rackcdn.com/centos-6.5-provisionerless.box"
    db.vm.network :private_network, ip: "192.168.2.3"
    db.vm.hostname = 'db'
    db.vm.provision :chef_client do |chef|
      chef.json = {
        :vagrant => {
          :ip => "192.168.2.3"
        }
      }
      chef.run_list = [
          "recipe[barbican-postgresql]"
      ]
    end

  end

  config.vm.define :api do |api|
    api.vm.box = "centos-6.5"
    api.vm.box_url = "https://ca9f9b62ae1b1e47c4f8-989e703834df24142dff402333777a7c.ssl.cf1.rackcdn.com/centos-6.5-provisionerless.box"
    api.vm.network :private_network, ip: "192.168.2.4"
    api.vm.hostname = 'api'
    api.vm.provision :chef_client do |chef|
      chef.json = {
        :vagrant => {
          :ip => "192.168.2.4"
        },
        :barbican => {
          :use_postgres => true,
          :discovery => {
            :enable_queue_search => true,
            :queue_search_query => 'node_group_tag:queue',
            :queue_ip_attribute => 'vagrant.ip',
            :enable_db_search => true,
            :db_search_query => 'node_group_tag:database',
            :db_ip_attribute => 'vagrant.ip'
          },
          :queue => {
            :enable => true,
            :rabbit_ha_queues => true
          }
        }
      }
      chef.run_list = [
          "recipe[barbican::search_discovery]",
          "recipe[barbican::api]"
      ]
    end
  end

  config.vm.define :worker do |worker|
    worker.vm.box = "centos-6.5"
    worker.vm.box_url = "https://ca9f9b62ae1b1e47c4f8-989e703834df24142dff402333777a7c.ssl.cf1.rackcdn.com/centos-6.5-provisionerless.box"
    worker.vm.network :private_network, ip: "192.168.2.5"
    worker.vm.hostname = 'worker'
    worker.vm.provision :chef_client do |chef|
      chef.json = {
        :vagrant => {
          :ip => "192.168.2.5"
        },
        :barbican => {
          :use_postgres => true,
          :discovery => {
            :enable_queue_search => true,
            :queue_search_query => 'node_group_tag:queue',
            :queue_ip_attribute => 'vagrant.ip',
            :enable_db_search => true,
            :db_search_query => 'node_group_tag:database',
            :db_ip_attribute => 'vagrant.ip'
          },
          :queue => {
            :enable => true,
            :rabbit_ha_queues => true
          }
        }
      }
      chef.run_list = [
          "recipe[barbican::search_discovery]",
          "recipe[barbican::worker]"
      ]
    end
  end

end
