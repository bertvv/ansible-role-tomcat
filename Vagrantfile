# vi: ft=ruby

require 'rbconfig'

VAGRANTFILE_API_VERSION = '2'
ROLE_NAME = 'tomcat'

hosts = [
  { distro: 'fedora23', ip: '192.168.56.7' },
  { distro: 'centos72', ip: '192.168.56.8' }
]

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = 'testf23' # 'bertvv/fedora23'

  hosts.each do |host|
    host_name = host[:distro] + '-' + ROLE_NAME

    config.vm.define host_name do |node|
      node.vm.box = 'bertvv/' + host[:distro]
      node.vm.hostname = host_name
      node.vm.network :private_network, ip: host[:ip]
      node.vm.provision 'ansible' do |ansible|
        ansible.playbook = 'test.yml'
      end
    end
  end
end
