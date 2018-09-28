Vagrant.configure("2") do |config|

  config.vm.box = "generic/ubuntu1804"
   
  config.vm.provider :libvirt do |domain|
             domain.memory = 1024
             domain.cpus = 2
  end
  
  
  config.vm.provision "shell" do |s|
      ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
      s.inline = <<-SHELL
        mkdir -p /root/.ssh
        echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
        echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
      SHELL
  end
  
  
  config.vm.define :nginxwebserver do |nginx|
      nginx.vm.hostname = "nginxwebserver"
      nginx.vm.network :private_network,
          :ip => '10.10.10.12',
          :libvirt__forward_mode => 'none'
      nginx.vm.provision "shell" do |s|
          s.inline = <<-SHELL
          apt-get install -y nginx
          SHELL
      end
  end
  
  config.vm.define :clientwebserver do |client|
      client.vm.hostname = "clientwebserver"
      client.vm.network :private_network,
  	:ip => '10.10.20.14',
  	:libvirt_forward_mode => 'none'
  end
  
  config.vm.define :firewallmachine do |fw|
      fw.vm.hostname = "firewallmachine"
      fw.vm.network :private_network,
          :ip => '10.10.20.13',
          :libvirt__forward_mode => 'none'
      fw.vm.network :private_network,
          :ip => '10.10.10.13',
          :libvirt__forward_mode => 'none'
  end

end

