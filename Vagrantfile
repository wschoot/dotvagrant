# vi: set ft=ruby:

Vagrant.configure("2") do |config|

  routes = `ip route`.lines.grep(/default via/)
  if routes.first.start_with?("default")
    iface = routes.first.split(" ")[4]
    puts "Detected " + iface + " as default interface"
  end

  config.vm.provider :libvirt do |vb|
    vb.memory = 4096
    vb.cpus = 4
  end
  config.vm.synced_folder ".", "/vagrant", type: "sshfs"

  # Uitgaande iface van je host
  config.vm.network "public_network", bridge: iface, dev: iface

  config.vm.define "ubuntu" do |ubuntu|
		ubuntu.vm.box = "generic/ubuntu1904"
		ubuntu.vm.hostname = "ubuntu.local"
  end

  config.vm.define "centos7" do |centos7|
		centos7.vm.box = "centos/7"
		centos7.vm.hostname = "centos7.local"
  end

  config.vm.provision :ansible do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.compatibility_mode = "2.0"
  end
end

