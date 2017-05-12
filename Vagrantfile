required_plugins = %w(vagrant-vbguest)
required_plugins.each do |plugin|
  system "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
end

Vagrant.configure("2") do |config|

  #This is the openshift host
  config.vm.define "openshift" do |openshift_config|
    openshift_config.vm.box = "centos/7"
    openshift_config.vm.hostname = "openshift"
    openshift_config.vm.synced_folder "./openshift", "/vagrant", rsync__exclude: ".git/,./scripts"
    openshift_config.vm.network "private_network", ip: "192.168.50.10", :netmask => "255.255.255.0",  auto_config: true
    openshift_config.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", "4096"]
    end
    openshift_config.vm.provision :shell, path: "scripts/bootstrap.sh"
    openshift_config.vm.provision :shell, path: "scripts/passwordAuthentication.sh"
    openshift_config.vm.provision "shell", inline: <<-SHELL
        sudo cp -r /vagrant/* /home/vagrant/
        #if [ "`systemctl is-active firewalld`" == "active" ]; then
        #  echo "Firewalld is actived"
        #else
        #  sudo systemctl enable firewalld
        #  sudo systemctl start firewalld
        #  sudo firewall-cmd --zone=public --add-port 80/tcp --permanent
        #  sudo firewall-cmd --zone=public --add-port 443/tcp --permanent
        #  sudo firewall-cmd --reload
        #  sudo firewall-cmd --list-ports
        #fi
    SHELL
    openshift_config.vm.provision :shell, path: "scripts/javaInstall.sh"

  end
  
end