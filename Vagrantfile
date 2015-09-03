BOX_NAME = 'ubuntu/trusty64'
DEBUG = 'v'
require 'rbconfig'
is_windows = (RbConfig::CONFIG['host_os'] =~ /mswin|mingw|cygwin/)

Vagrant.configure('2') do |config|
  config.vm.box = BOX_NAME
  #mysql
  config.vm.network :forwarded_port, guest: 3306, host: 3306, host_ip: "127.0.0.1"
  #nginx
  config.vm.network :forwarded_port, guest: 80, host: 8080, host_ip: "127.0.0.1"
  #rabbitmq management
  config.vm.network :forwarded_port, guest: 15672, host: 15672, host_ip: "127.0.0.1"
  # rabbitmq amqp
  config.vm.network :forwarded_port, guest: 5672, host: 5672, host_ip: "127.0.0.1"
  #redis
  config.vm.network :forwarded_port, guest: 6379, host: 6379, host_ip: "127.0.0.1"
  if is_windows
    # Provisioning configuration for shell script.
    config.vm.provision "shell" do |sh|
      sh.path = 'windows.sh'
      sh.args = 'playbook.yml'
    end
  else
    config.vm.provision 'ansible' do |ansible|
      ansible.playbook = 'playbook.yml'
      ansible.limit = 'all'
      ansible.sudo = true
      ansible.host_key_checking = false
      ansible.verbose = DEBUG
    end
  end
  config.vm.define 'nstack' do |c|
    c.vm.host_name = 'nstack'
  end

  config.vm.provider 'virtualbox' do |v|
    v.memory = 2048
    v.cpus = 2
  end
end
