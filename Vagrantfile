# -*- mode: ruby -*-
# vi: set ft=ruby ts=2 sw=2 expandtab:

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "chef/fedora-20"
  config.vm.box_check_update = false

  # Desabilita a atualização automática do VirtualBox Guest Additions
  # (feita pelo plugin vbguest - https://github.com/dotless-de/vagrant-vbguest)
  #config.vbguest.auto_update = false

  # usa a variável de ambiente SISLEGIS_PORT para determinar a porta do sislegis no HOST
  # se ela não estiver definida, utiliza a porta 8080 (default no WildFly)
  sislegis_port = ENV['SISLEGIS_PORT'] ? ENV['SISLEGIS_PORT'] : 8080

  # Exporta as portas utilizadas pelo WildFly
  config.vm.network :forwarded_port, guest: 8080, host: sislegis_port
  config.vm.network :forwarded_port, guest: 9990, host: 9990

  config.vm.provider "virtualbox" do |v|
    #v.gui = true
    v.memory = 2048
  end
end
