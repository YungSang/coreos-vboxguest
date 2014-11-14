# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "coreos-vboxguest"

  config.vm.box = "yungsang/coreos-alpha"

  config.vm.box_version = "1.3.3"

  if ARGV[0] == "up" || ARGV[0] == "provision" then
    config.vm.network :private_network, ip: "192.168.33.10"

    config.vm.synced_folder ".", "/tmp/vagrant", id: "core", type: "nfs", mount_options: ["nolock", "vers=3", "udp"]

    config.vm.provision :docker do |d|
      d.build_image "/tmp/vagrant/", args: "-t yungsang/vboxguest"
      d.run "yungsang/vboxguest", args: "--rm -v /usr/share/oem:/target",
        auto_assign_name: false, daemonize: false
    end

    config.vm.provision :shell do |s|
      s.inline = <<-EOT
        sudo cp -R /usr/lib/modules/`uname -r`/* /usr/share/oem/lib/modules/`uname -r`/
        sudo depmod -b /usr/share/oem
        sudo umount /tmp/vagrant
      EOT
    end
  end

  config.vm.provision :shell, run: "always" do |s|
    s.inline = <<-EOT
      sudo modprobe -d /usr/share/oem vboxguest
      sudo modprobe -d /usr/share/oem vboxsf
      sudo lsmod | grep vboxsf
    EOT
  end

  # Set a shared folder to the current folder (You can set --hostpath to anywhere you want.)
  config.vm.provider :virtualbox do |vb, override|
    vb.customize [
      "sharedfolder", "add", :id,
      "--name", "coreos",
      "--hostpath", File.expand_path(File.dirname(__FILE__))
    ]
  end

  config.vm.provision :shell, run: "always" do |s|
    s.inline = <<-EOT
      sudo mkdir -p /home/core/vagrant
      sudo /usr/share/oem/sbin/mount.vboxsf -o uid=`id -u core`,gid=`id -g core` coreos /home/core/vagrant
      EOT
  end
end
