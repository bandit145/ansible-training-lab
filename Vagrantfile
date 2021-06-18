Vagrant.configure("2") do |config|

	config.vm.provider :virtualbox do |virtualbox|
		virtualbox.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
		virtualbox.gui = false
		virtualbox.memory = 2048
	end

	(1..4).each do |num|
		config.vm.define "server#{num}" do |server|
		  server.vm.box = "bento/centos-8"
		  server.vm.network "private_network", ip: "192.168.50.#{num + 1}"
		end
	end

	config.vm.define "ansible-server" do |ansible|
		ansible.vm.box = "bento/centos-8"
		ansible.vm.network "private_network", ip: "192.168.50.6"
		ansible.vm.provision "shell", inline: "hostnamectl set-hostname control"
		ansible.vm.provision "shell", inline: "echo '192.168.50.2 server1\n192.168.50.3 server2\n192.168.50.4 server3\n192.168.50.5 server4' >> /etc/hosts"
		ansible.vm.provision "shell", inline: "sudo yum -y install epel-release && sudo yum -y install python3 python3-pip python3-virtualenv"
		ansible.vm.provision "shell", inline: "virtualenv -p python3 venv && venv/bin/pip3 install ansible==2.9.20"
	end
end
