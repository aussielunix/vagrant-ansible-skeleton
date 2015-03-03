# -*- mode: ruby -*-
# vi: set ft=ruby :

# Find the current vagrant directory & create additional vars from it
vagrant_dir = File.expand_path(File.dirname(__FILE__))
settings_file = vagrant_dir + "/settings.yml"

puts
puts "Loading settings file: #{settings_file}"
puts

# Include config from settings file
require 'yaml'
vconfig = YAML::load_file(settings_file)

# Set box configuration options
box_ip   = vconfig['box_ip']
box_name = vconfig['box_name']
box_ram  = vconfig['box_ram']
box_cpus = vconfig['box_cpus']
box_base = vconfig['box_base']

Vagrant.configure("2") do |config|
  # Base VM OS configuration - set in settings.yml
  config.vm.box = box_base
  config.ssh.insert_key = false

  # General VirtualBox VM configuration.
  config.vm.provider :virtualbox do |v|
    v.customize ["modifyvm", :id, "--memory", box_ram]
    v.customize ["modifyvm", :id, "--cpus", box_cpus]
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  # Provision
  config.vm.define box_name do |sshkeys|
    sshkeys.vm.hostname = box_name
    sshkeys.vm.network :private_network, ip: box_ip
    sshkeys.vm.provision "ansible" do |ansible|
      ansible.playbook = vconfig["ansible_playbook"]
      ansible.sudo = vconfig["ansible_sudo"]
      ansible.extra_vars = {
        ansible_ssh_user: 'vagrant',
        ansible_ssh_private_key_file: "~/.vagrant.d/insecure_private_key"
      }
      unless vconfig['ansible_verbosity'] == ''
        ansible.verbose = vconfig['ansible_verbosity']
      end
    end
  end
  # to speed up subsequent rebuilds install vagrant-cachier
  # to cache packages in your ~/.vagrant.d/cache directory
  # https://github.com/fgrehm/vagrant-cachier
  #   `vagrant plugin install vagrant-cachier`
  unless ENV['DISABLE_VAGRANT_CACHE']
    if Vagrant.has_plugin?("vagrant-cachier")
      # Configure cached packages to be shared between instances of the same base box.
      # More info on http://fgrehm.viewdocs.io/vagrant-cachier/usage
      config.cache.scope = :box
    else
      puts "to speed up subsequent rebuilds install vagrant-cachier"
    end
  end
end
