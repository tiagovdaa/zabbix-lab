ZABBIX_VERSION_MAJOR = "5.4"
ZABBIX_VERSION_MINOR = "5.4.2"
POSTGRES_VERSION = "13"
PHP_TIMEZONE = "America/Sao_Paulo"
$DEFAULT_NETWORK_INTERFACE = `ip route | awk '/^default/ {printf "%s", $5; exit 0}'`

Vagrant.configure("2") do |config|

  config.vm.provider "virtualbox" do |vbox|  
    vbox.memory = 2048  
    vbox.cpus = 2
    vbox.name = "zabbix"
  end

	config.vm.define "zabbix" do |zabbix|

		config.vm.box = "centos/8"

		zabbix.vm.hostname = "zabbix"
    zabbix.vm.network "public_network", bridge: "#$DEFAULT_NETWORK_INTERFACE"
		#zabbix.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
		#zabbix.vm.network "forwarded_port", guest: 5432 , host: 5432, host_ip: "127.0.0.1"
		#zabbix.vm.network "forwarded_port", guest: 10050 , host: 10050, host_ip: "127.0.0.1"
		#zabbix.vm.network "forwarded_port", guest: 10051 , host: 10051, host_ip: "127.0.0.1"
    zabbix.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbook.yaml"
      ansible.extra_vars = {
        username: "#{ENV['USERNAME'] || `whoami`}",
      }
    end
    $script = <<-SCRIPT
    echo "configured networks: \n" 
    ip -br a |grep -i up|awk '{print "INTERFACE:",$1,"ADDRESS:",$3}'
    SCRIPT
    zabbix.vm.provision "shell" do |shell|
      shell.inline = $script
    end
	end
end
  