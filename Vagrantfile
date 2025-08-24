# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'
config_data = YAML.load_file('vm_config.yaml')


Vagrant.configure("2") do |config|

  config_data['vms'].each do |vm|
    config.vm.define vm['name'] do |host|

      host.vm.box = config_data['box']
      host.vm.hostname = vm['hostname']
      host.vm.box_check_update = true

      host.vm.provider "virtualbox" do |vb|
        vb.name = vm['hostname']
        vb.cpus = config_data['cpus']
        vb.memory = config_data['memory']
      end
      
      host.vm.provision "shell", inline: <<-SHELL

        set -e
        
        apt update
        apt upgrade -y
        
        USERNAME="#{config_data['login']}"
        PASSWORD_HASH="#{config_data['password']}"
        PUBLIC_KEY="#{config_data['public_key']}"
        
        # add user
        if ! id -u $USERNAME >/dev/null 2>&1; then
          useradd -m -s /bin/bash $USERNAME
        fi        
        echo "$USERNAME:$PASSWORD_HASH" | chpasswd -e
        usermod -aG sudo $USERNAME
        
        # add ssh public key to authorized_keys
        mkdir -p /home/$USERNAME/.ssh
        chmod 700 /home/$USERNAME/.ssh
        echo "$PUBLIC_KEY" > /home/$USERNAME/.ssh/authorized_keys
        chmod 600 /home/$USERNAME/.ssh/authorized_keys
        chown -R $USERNAME:$USERNAME /home/$USERNAME/.ssh
        
        echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME
        chmod 440 /etc/sudoers.d/$USERNAME

        shutdown -r now

      SHELL
    end

  end
end