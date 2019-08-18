# vi: ft=ruby

require 'rbconfig'

ROLE_NAME = 'mariadb'
VAGRANTFILE_API_VERSION = '2'

distros = [
  { name: 'centos', ip: '10', box: 'bento/centos-7.6', python: 'python' },
  { name: 'fedora', ip: '11', box: 'bento/fedora-30' , python: 'python3'},
]

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  distros.each do |distro|
    srv_name = 'srv' + distro[:name]
    config.vm.define srv_name do |node|
      node.vm.hostname = srv_name
      node.vm.box = distro[:box]

      node.vm.network :private_network,
        ip: '192.168.56.' + distro[:ip]

      node.vm.provision 'ansible' do |ansible|
        ansible.host_vars = {
          srv_name => { "ansible_python_interpreter" => "/usr/bin/" + distro[:python] }
        }
        ansible.compatibility_mode = '2.0'
        ansible.playbook = 'test.yml'
      end
    end
  end
end

