# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.define "cf-install"
  config.vm.box = "precise64"
  
  config.vm.provider :virtualbox do |v|
    config.vm.box_url = "http://files.vagrantup.com/precise64.box"
    v.customize ["modifyvm", :id, "--memory", "2048"]
  end

  config.vm.provider :vmware_fusion do |v|
    config.vm.box_url = "http://files.vagrantup.com/precise64_vmware.box"
  end

  # forward requests to port 8181 on the host to port 80 on the guest, which
  # is the port that the Cloud Controller is listening on 
  config.vm.network :forwarded_port, guest: 80, host: 8181

  config.berkshelf.enabled = true

  config.vm.provision :chef_solo do |chef|
    chef.add_recipe 'apt::default'
    chef.add_recipe 'git'
    chef.add_recipe 'chef-golang'
    chef.add_recipe 'ruby_build'
    chef.add_recipe 'rbenv::user'
    chef.add_recipe 'java::openjdk'
    chef.add_recipe 'sqlite'
    chef.add_recipe 'mysql::server'
    chef.add_recipe 'postgresql::server'
    
    chef.add_recipe 'rbenv-alias'
    chef.add_recipe 'cloudfoundry::warden'
    chef.add_recipe 'cloudfoundry::dea'
    chef.add_recipe 'cloudfoundry::uaa'

    chef.json = {
      'rbenv' => {
        'user_installs' => [ { 
          'user' => 'vagrant', 
          'global' => '1.9.3-p392',
          'rubies' => [ '1.9.3-p392' ],
          'gems' => {
            '1.9.3-p392' => [
              { 'name' => 'bundler' }
            ]
          },
        } ]
      },
      'rbenv-alias' => {
        'user_rubies' => [ {
          'user' => 'vagrant',
          'installed' => '1.9.3-p392',
          'alias' => '1.9.3'
        } ]
      },
      'mysql' => {
        'server_root_password' => '',
        'server_repl_password' => '',
        'server_debian_password' => ''
      },
      'postgresql' => {
        'password' => {
          'postgres' => ''
        }
      }
    }
  end
end
