# -*- mode: ruby -*-
# vi: set ft=ruby :

$puppet_module_script = <<EOF
  mkdir -p /etc/puppet/modules
  (puppet module list | grep puppetlabs-postgresql) ||
     (puppet module install puppetlabs-postgresql)
  (puppet module list | grep puppetlabs-stdlib) ||
     (puppet module install puppetlabs-stdlib)
  (puppet module list | grep puppetlabs-apache) ||
     (puppet module install puppetlabs-apache)
EOF

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty32"
  # Only tested on VirtualBox, so let's mandate that (and also give the
  # VM a name)
  config.vm.provider :virtualbox do |vb|
    vb.name = "links-vagrant-vm"
  end

  # Shared folder should be sync, not "."
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder "sync", "/vagrant"

  # Provisioning: we firstly want to ensure that we install the correct
  # Puppet modules (in particular, apache, postgres, opam)
  # (might we get away with just adding these as deps and only requiring
  # the Links one here?)
  config.vm.provision "shell", inline: $puppet_module_script

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision :puppet do |puppet|
    puppet.manifests_path = "puppet/manifests"
    puppet.manifest_file = "site.pp"
    # FIXME: OBVIOUSLY THIS NEEDS TO CHANGE!!!!!
     puppet.module_path = "~/.puppetlabs/etc/code/modules"
  end

  config.vm.network :forwarded_port, guest: 80, host: 8080
end
