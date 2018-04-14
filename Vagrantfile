['vagrant-bindfs', 'vagrant-vbguest', 'vagrant-hostmanager'].each do |p|
  if !Vagrant.has_plugin? "#{p}"
    abort "#{p} plugin missing, install using: vagrant plugin install #{p}"
  end
end

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.box_check_update = true
  config.vm.hostname = "dev.vm"

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.cpus = 2
    vb.memory = 2048
    vb.name = "dev.vm"
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end

  config.vm.network "private_network", type: "dhcp"
  config.vm.synced_folder "../code", "/home/vagrant/code", type: "nfs"
  config.bindfs.bind_folder "/home/vagrant/code", "/home/vagrant/code"

  config.ssh.forward_agent = true
  config.ssh.shell = %{bash -c 'BASH_ENV=/etc/profile exec bash'}

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  subdomains = ["api", "www"]

  config.vm.provision "setup", type: "ansible" do |ansible|
    ansible.compatibility_mode = '2.0'
    ansible.galaxy_role_file = 'provisioning/requirements.yaml'
    ansible.playbook = "provisioning/setup.yaml"
  end

  config.vm.provision "hostmanager", run: "always"

end
