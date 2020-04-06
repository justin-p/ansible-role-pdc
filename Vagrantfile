# Require YAML module
require 'yaml'
 
# Read YAML file with box details
inventory = YAML.load_file('tests/inventory.yml')

Vagrant.configure("2") do |config|
  config.vm.define "dc" do |dc|
    inventory['all']['children']['primarydomaincontroller']['hosts'].each do |server,details|
      dc.vm.box      = details['vagrant_box']
      dc.vm.hostname = server
      dc.vm.network :private_network, ip: details['ansible_host']
      inventory['all']['vars']['vagrant_ports'].each do |protocol,details|
        dc.vm.network :forwarded_port, guest: details['guest'], host: details['host'], id: protocol
      end
      dc.vm.provider :virtualbox do |v|
        v.name = File.basename(File.dirname(__FILE__)) + "_" + server + "_" + Time.now.to_i.to_s
        v.gui = false
        v.memory = 2048
        v.cpus = 2
      end
    end
  end
  inventory['all']['children']['primarydomaincontroller']['hosts'].each do |server,details|
    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "tests/test.yml"
      ansible.limit = "all"
      ansible.inventory_path = "tests/inventory.yml"
      ansible.verbose = "-vvvv"
    end  
  end
end
