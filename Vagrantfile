# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
  config.vm.provider 'virtualbox' do |vb|
    vb.gui = false
    vb.memory = '2048'
  end

  config.vm.define('DB') do |db|
    db.vm.box = 'ubuntu/impish64'
    db.vm.network 'private_network', ip: '192.168.56.10'
    db.vm.network 'forwarded_port', guest: 3360, host: 3360

    db.vm.provision 'ansible' do |ansible|
      ansible.playbook = 'provisioning/playbook.yml'
      ansible.become = true
      ansible.compatibility_mode = '2.0'
    end
  end
end
