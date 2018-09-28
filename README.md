# Testport80
Additional steps implemented on the nodes include the IP routes on the nginx and client server and the FORWARding being enabled on the firewall machine:

config.vm.define :nginxwebserver do |nginx|
      nginx.vm.hostname = "nginxwebserver"
      nginx.vm.network :private_network,
          :ip => '10.10.10.12',
          :libvirt__forward_mode => 'none'
      nginx.vm.provision "shell" do |s|
          s.inline = <<-SHELL
          apt-get install -y nginx
          ip route add 10.10.20.0/24 via 10.10.10.13
          SHELL
      end
  end
  
  config.vm.define :clientwebserver do |client|
      client.vm.hostname = "clientwebserver"
      client.vm.network :private_network,
  	:ip => '10.10.20.14',
  	:libvirt_forward_mode => 'none'
      client.vm.provision "shell" do |s|
          s.inline = <<-SHELL
	  ip route add 10.10.10.0/24 via 10.10.20.13
          SHELL
      end

  end
  
  config.vm.define :firewallmachine do |fw|
      fw.vm.hostname = "firewallmachine"
      fw.vm.network :private_network,
          :ip => '10.10.20.13',
          :libvirt__forward_mode => 'none'
      fw.vm.network :private_network,
          :ip => '10.10.10.13',
          :libvirt__forward_mode => 'none'
      fw.vm.provision "shell" do |s|
          s.inline = <<-SHELL
          sysctl -w net.ipv4.ip_forward=1
          SHELL
      end

  end




