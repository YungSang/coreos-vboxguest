# VirtualBox Guest Additions for CoreOS

Build VirtualBox Guest Additions for CoreOS and setup a shared folder with Vagrant

**This is just an experimental project to prove the possibility.**  
I know this is not a good idea, but you may be interested in it for a particular case.

## Requirements

- [VirtualBox](https://www.virtualbox.org/) = v4.3.18
- [Vagrant](https://www.vagrantup.com/) >= v1.6.5

## How to Use

```
$ git clone https://github.com/YungSang/coreos-vboxguest.git
$ cd coreos-vboxguest
$ vagrant up
```

## Limitations

Currently it supports [CoreOS 494.0.0](https://github.com/coreos/manifest/releases/tag/v494.0.0)
  with [the latest CoreOS linux kernel 3.17.2](https://github.com/coreos/linux/tree/coreos/v3.17.2)
  and VirtualBox v4.3.18.
