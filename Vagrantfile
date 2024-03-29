# -*- mode: ruby -*-
# vi: set ft=ruby :

# load configs
require 'yaml'
current_dir    = File.dirname(File.expand_path(__FILE__))
configs        = YAML.load_file("#{current_dir}/config.yml")
vagrant_config = configs['configs']

#require necessary plugins
required_plugins = %w( vagrant-hostmanager vagrant-vbguest )
required_plugins.each do |plugin|
  system "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
end

Vagrant.configure(2) do |config|
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.include_offline = true

  config.vm.define "server" do |s|
    #config.vm.box = "ubuntu/xenial64" # 16.04
    s.vm.box = "ubuntu/bionic64" # 18.04
    # set memory to 2048m
    s.vm.provider "virtualbox" do |vb|
      vb.memory = vagrant_config['server']['memory']
      vb.cpus = vagrant_config['server']['cpus']
    end

    s.vm.synced_folder ".", "/vagrant", disabled: true
    s.vm.synced_folder "ansible_vagrant", "/vagrant/ansible_vagrant", create: true, owner: "vagrant", group: "vagrant", mount_options: ["dmode=775,fmode=775"]

    # auto update guest additions
    s.vbguest.auto_update = true

    # vagrant-hostmanager is necessary to update /etc/hosts on hosts and guests
    s.vm.network "private_network", ip: vagrant_config['server']['ip']
    s.vm.hostname = vagrant_config['server']['domain']
    #s.hostmanager.aliases = %w(www.yourdomain.lokal)

    # Run Ansible from the Vagrant VM
    s.vm.provision "ansible_local" do |ansible|
      ansible.install = true
      ansible.install_mode = :default
      ansible.playbook = "ansible_vagrant/playbook.yml"
      ansible.galaxy_role_file = "ansible_vagrant/requirements.yml"
      ansible.extra_vars = { ansible_python_interpreter:"/usr/bin/python3" }
    end
  end
end
