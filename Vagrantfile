# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.ssh.username = "vagrant"
  config.ssh.password = "vagrant"

  config.vm.define "apt-cacher" do |node|
    node.vm.provider "docker" do |docker|
      docker.name = "apt-cacher"
      docker.create_args = ["--hostname=apt-cacher", "--network=elasticsearch"]
      docker.build_dir = "apt-cacher"
    end
  end
end
