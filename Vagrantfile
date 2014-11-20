# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "coreos-vboxguest"

  config.vm.box = "yungsang/coreos-alpha"

  config.vm.box_version = ">= 1.3.1, <= 1.4.0"

  if ARGV[0] == "up" || ARGV[0] == "provision" then
    config.vm.provision :docker do |d|
      d.pull_images "yungsang/coreos-vboxguest:3.17.2-4.3.18"
      d.run "yungsang/coreos-vboxguest:3.17.2-4.3.18", args: "--rm -v /usr/share/oem:/target",
        auto_assign_name: false, daemonize: false
    end

    config.vm.provision :shell do |s|
      s.inline = <<-EOT
        sudo cp -R /usr/lib/modules/`uname -r`/* /usr/share/oem/lib/modules/`uname -r`/
        sudo depmod -b /usr/share/oem
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
      sudo umount /home/core/vagrant 2> /dev/null || true
      sudo mkdir -p /home/core/vagrant
      sudo /usr/share/oem/sbin/mount.vboxsf -o uid=`id -u core`,gid=`id -g core` coreos /home/core/vagrant
      EOT
  end
end
