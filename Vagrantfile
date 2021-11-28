# -*- mode: ruby -*-
# vi: set ft=ruby :

def provisioned?(vm_name: 'DB', provider: 'virtualbox')
  File.exist?(".vagrant/machines/#{vm_name}/#{provider}/action_provision")
end

Vagrant.configure('2') do |config|
  config.vm.provider 'virtualbox' do |vb|
    vb.gui = false
    vb.memory = '2048'
  end

  config.vm.define('DB') do |db|
    db.vm.box = 'ubuntu/impish64'
    db.vm.network 'private_network', ip: '192.168.56.10'
    db.vm.network 'forwarded_port', guest: 3360, host: 3360

    if provisioned?
      db.vm.synced_folder './mysql-data', '/vagrant/data',
                          owner: 'mysql', group: 'mysql'
    else
      db.vm.synced_folder './mysql-data', '/vagrant/data'
    end

    db.vm.provision 'ansible' do |ansible|
      ansible.playbook = 'provisioning/playbook.yml'
      ansible.become = true
      ansible.compatibility_mode = '2.0'
    end

    db.trigger.after :up, :reload do |tr|
      tr.name = 'Start mysqld'
      tr.info = 'starting mysql'
      tr.run_remote = { inline: 'sudo systemctl start mysql.service' }
    end
  end
end
