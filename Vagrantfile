Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.provision :shell, path: "provision/HelloWorld.sh" 
  config.vm.provision :shell, inline: "echo Helo,World! from INLINE"
end
