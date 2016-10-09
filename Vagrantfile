# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  # disable the synced folder, we don't need it and it will just take time
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    DEBIAN_FRONTEND=noninteractive apt-get install -y python python-apt ca-certificates
  SHELL

  config.vm.define "obnam" do |obnam|
    obnam.vm.box = "debian/jessie64"
    obnam.vm.hostname = "obnam.vagrant.example.com"
  end

  config.vm.define "client" do |client|
    client.vm.box = "debian/jessie64"
    client.vm.hostname = "client.vagrant.example.com"

    # only run ansible provision once, with limit all, on the last host
    # otherwise vagrant will run ansible with limit to each provisioned
    # host, forgeting all the facts of the other hosts
    client.vm.provision "ansible" do |ansible|
      ansible.verbose = "v"
      ansible.limit = "all"
      ansible.playbook = "example.yml"
    end
  end

end
