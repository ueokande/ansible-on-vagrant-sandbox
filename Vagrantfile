# -*- mode: ruby -*-
# vi: set ft=ruby :

def vanilla_container(config, name, &proc)
  config.vm.define name do |node|
    node.vm.provider "docker" do |docker|
      docker.name = name
      docker.create_args = ["--hostname=#{name}", "--network=elasticsearch"]
      docker.build_dir = "vanilla"
      docker.has_ssh = true

      proc.call(docker) if block_given?
    end
  end
end

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

  (1..4).each do |i|
    vanilla_container config, "elasticsearch-master-#{i}"
  end
  (1..6).each do |i|
    vanilla_container config, "elasticsearch-data-#{i}"
  end
  vanilla_container config, "elasticsearch-client" do |docker|
    docker.expose = [9200]
  end
end
