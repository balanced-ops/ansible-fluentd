# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "precise64"
  config.hostmanager.enabled = true # Must install: vagrant plugin install vagrant-hostmanager

  config.vm.define 'fluentd.server1' do |node|
    node.vm.network :private_network, :ip => '10.20.3.11'
    config.vm.host_name = 'fluentd.server1'
  end

  config.vm.define 'fluentd.server2' do |node|
    node.vm.network :private_network, :ip => '10.20.3.12'
    config.vm.host_name = 'fluentd.server2'
  end

  config.vm.define 'fluentd.client1' do |node|
    node.vm.network :private_network, :ip => '10.20.3.21'
    config.vm.host_name = 'fluentd.client1'
  end

  config.vm.provision :ansible do |ansible|
    ansible.playbook       = './tests/test.yml'
    ansible.inventory_path = './tests/vagrant-inventory'
    ansible.extra_vars = {
        ansible_ssh_user: 'vagrant',
        sudo: true
    }
    ansible.host_key_checking = false
    ansible.groups = {
        'fluentd-servers' => ['fluentd.server1', 'fluentd.server2'],
        'fluentd-clients' => ['fluentd.client1'],
    }

    # Debugging helpers
    ansible.verbose = 'vvvv'
    #ansible.hosts = 'all'
  end

end
