# -*- mode: ruby -*-
# vi: set ft=ruby :
boxes = [
  { :name => :mongodbserver, :ip => '192.168.33.10', :ssh_port => 2202, :cpus => 1, :mem => 2048, :mac => "720002691321" },
]
# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    boxes.each do |opts|
        config.vm.synced_folder "./sync", "/vagrant", type: "rsync"
        config.vm.define opts[:name] do |config|
          config.vm.box = "CentOS-7.0-1406-x86_64"
          config.vm.box_url = "http://www.cngrgroup.com/VirtualBoxImage/CentOS-7.0-1406-x86_64.box"
          # config.vm.box_url = "/Users/psenger/VirtualMachineImages/CentOS-7.0-1406-x86_64.box"
          config.vm.network "private_network", ip: opts[:ip], guest: 22, host: opts[:ssh_port], netmask: "255.255.255.0" #, auto_config: false
          config.vm.hostname = "%s" % opts[:name].to_s
          config.vm.provision "puppet" do |puppet|
                # puppet.options = "--verbose --debug"
                puppet.manifests_path = "puppet"
                puppet.manifest_file = "mongodb.pp"
          end
          config.vm.provider "virtualbox" do |vb|
                # Use VBoxManage to customize the VM
                vb.customize ["modifyvm", :id, "--cpus", opts[:cpus] ] if opts[:cpus]
                vb.customize ["modifyvm", :id, "--memory", opts[:mem] ] if opts[:mem]
          end
       end
    end
end