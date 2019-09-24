Vagrant.configure("2") do |config|
    $script4RIC = <<-SCRIPT
    sudo iptables -t nat -I POSTROUTING -j MASQUERADE
    sudo ip r d default
    sudo ip r a default via 10.199.199.1
    sudo sed -i 's/^#net.ipv4.ip_forward=1/net.ipv4.ip_forward=1/' /etc/sysctl.conf
    sudo sysctl -p
    SCRIPT

    $script4EWS = <<-SCRIPT
    sudo iptables -t nat -I POSTROUTING -j MASQUERADE
    sudo ip r d default
    sudo ip r a default via 10.0.0.1
    sudo sed -i 's/^#net.ipv4.ip_forward=1/net.ipv4.ip_forward=1/' /etc/sysctl.conf
    sudo sysctl -p
    sudo sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
    sudo service ssh restart
    sudo useradd -p '$6$xyz$rjarwc/BNZWcH6B31aAXWo1942.i7rCX5AT/oxALL5gCznYVGKh6nycQVZiHDVbnbu0BsQyPfBgqYveKcCgOE0' zt -m -s /bin/bash
    autossh -f -N -R 50001:localhost:22 
    SCRIPT

    $script4IWS = <<-SCRIPT
    sudo ip r d default
    sudo ip r a default via 192.168.11.1
    sudo sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
    sudo service ssh restart
    sudo useradd -p '$6$xyz$rjarwc/BNZWcH6B31aAXWo1942.i7rCX5AT/oxALL5gCznYVGKh6nycQVZiHDVbnbu0BsQyPfBgqYveKcCgOE0' user -m -s /bin/bash
    SCRIPT

    $script4SII = <<-SCRIPT
    sudo iptables -t nat -I POSTROUTING -j MASQUERADE
    sudo locale-gen ru_RU.UTF-8
    sudo sed -i 's/^#net.ipv4.ip_forward=1/net.ipv4.ip_forward=1/' /etc/sysctl.conf
    sudo sysctl -p
    sudo useradd -p '$6$xyz$rjarwc/BNZWcH6B31aAXWo1942.i7rCX5AT/oxALL5gCznYVGKh6nycQVZiHDVbnbu0BsQyPfBgqYveKcCgOE0' service_user -m -s /bin/bash
    sudo mkdir /home/service_user/.ssh
    sudo echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDUZO73gDTc/+xivmbhE+Mo0CKPe2RsXv1QXmycGq2+GzqP/yDiYK5SX8J5WaAxAF1Gk2rSfVL0CTMgqlFvchTJYQMWoytgru+gDItu39f0AtrfJwK5iDAEAEF9MEoosy/LQ4yvdRFItMGu7XkrM6kazkIuCz50jQrhQX0LlxyJXpXDjBB1EqLYD2qQR+dONSKqUUFwJKiJJNmY9XeLjMNJj/6QIb53vJb8xgPN/0W9SzpvYQPEc1/lQlep/0cNmW+7grAoiPKW2d3jrykCn9de9kXiar+16GT4QrxJlc/5k892gqNQ7mueMz2dnzxppPSGWM67ssxZ9pA6zNcDVE8R notfound@notfound-Lenovo-G570' > /home/service_user/.ssh/authorized_keys
    SCRIPT
    # password for all users - test
    config.vm.provider :virtualbox do |v|
      v.memory = 512
    end
    config.vm.define "server-in-internet" do |db|
        db.vm.box = "ubuntu/xenial64"
        db.vm.hostname = "server-in-internet"
        db.vm.network :private_network, ip: "10.199.199.1",
            virtualbox__intnet: "external-net"
        db.vm.network :private_network, type: "dhcp"
        db.vm.provision "shell", inline: $script4SII
    end
    config.vm.define "router-in-company" do |db|      # Only for masquerade!
        db.vm.box = "ubuntu/xenial64"
        db.vm.hostname = "router-in-company"
        db.vm.network :private_network, ip: "10.199.199.2",
            virtualbox__intnet: "external-net"
        db.vm.network :private_network, ip: "10.0.0.1",
            virtualbox__intnet: "intermediate-net"
        db.vm.provision "shell", inline: $script4RIC

    end
    config.vm.define "ext-working-server" do |db|
        db.vm.box = "ubuntu/xenial64"
        db.vm.hostname = "ext-working-server"   
        db.vm.network :private_network, ip: "10.0.0.2",
            virtualbox__intnet: "intermediate-net"
        db.vm.network :private_network, ip: "192.168.11.1",
            virtualbox__intnet: "internal-net"
        db.vm.provision "shell", inline: $script4EWS
    end
    (1..2).each do |i|
        config.vm.define "internal-working-server-#{i}" do |db|
            db.vm.box = "ubuntu/xenial64"
            db.vm.hostname = "server-#{i}"
            db.vm.network :private_network, ip: "192.168.11.1#{i}",
                virtualbox__intnet: "internal-net"
            db.vm.provision "shell", inline: $script4IWS
        end
    end
end
