# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"
#Fix for 100% cpu utilization maybe?
$enable_serial_logging = false

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.ssh.insert_key = true
  config.ssh.forward_agent = true
  config.vm.box = 'ubuntu/trusty64'

  # Development VM
  config.vm.define "dev", primary: true do |dev|
    dev.vm.network :private_network, ip: "192.168.111.101"
    dev.vm.provision "ansible" do |ansible|
      ansible.playbook = 'provisioning/dev.yml'
      ansible.verbose = 'vvv'
    end
  end

  # Production VM
  config.vm.define "prod", autostart: false do |prod|
    prod.vm.network :private_network, ip: "192.168.111.101"
    prod.vm.provision "ansible" do |ansible|
      ansible.playbook = 'provisioning/prod.yml'
      #ansible.verbose = 'vvv'
      ansible.vault_password_file = "vault_password.txt"
    end
  end

  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
  end

  config.vm.synced_folder "~/Projects/rpavlov.com", "/rpavlov.com", nfs: true,
                            nfs_export: true, nfs_udp: true, nfs_version: 3,
                            :mount_options => ['nolock,vers=3,udp,noatime,actimeo=1']
  config.vm.synced_folder "~/Projects/whythis.life", "/whythis.life", nfs: true,
                            nfs_export: true, nfs_udp: true, nfs_version: 3,
                            :mount_options => ['nolock,vers=3,udp,noatime,actimeo=1']

  # Use vagrant-cachier to cache apt-get, gems and other stuff across machines
  # Also consider using vagrant-exec, vagrant-faster and vagrant-omnibus
  if Vagrant.has_plugin?('vagrant-cachier')
    config.cache.scope = :box
    # config.cache.synced_folder_opts = {
    #   type: :nfs,
    #   # The nolock option can be useful for an NFSv3 client that wants to avoid the
    #   # NLM sideband protocol. Without this option, apt-get might hang if it tries
    #   # to lock files needed for /var/cache/* operations. All of this can be avoided
    #   # by using NFSv4 everywhere. Please note that the tcp option is not the default.
    #   mount_options: ['rw', 'vers=3', 'tcp', 'nolock']
    # }
  else
    puts "Run `vagrant plugin install vagrant-cachier` to reduce caffeine intake when provisioning"
  end

end
