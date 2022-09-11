# Vagrant-IAC
Criando VMs com Vagrant utilizando IAC e configurando docker para cada VM em código.

## Vagrantfile

Cada **VM** a ser criada estamos passando como argumento em "machines", repare que as **configurações estão dentro de chaves** e basicamente atribuimos os dados a tags como fossem variáveis, a primeira informação antes das chaves, claro, é o nome.
```bash
machines = {
  "node01" => {"ip" => "192.168.0.10", "memory" => "1024", "cpu" => "1", "image" => "bento/ubuntu-22.04"},
  "node02" => {"ip" => "192.168.0.10", "memory" => "1024", "cpu" => "1", "image" => "bento/ubuntu-22.04"},
  "node03" => {"ip" => "192.168.0.10", "memory" => "1024", "cpu" => "1", "image" => "bento/ubuntu-22.04"},
  "node04" => {"ip" => "192.168.0.10", "memory" => "1024", "cpu" => "1", "image" => "bento/ubuntu-22.04"}
}

Vagrant.configure("2") do |config|

  machines.each do |name, conf|
    config.vm.define "#{name}" do |machine|
      machine.vm.box = "#{conf["image"]}"
      machine.vm.hostname = "#{name}"
      machine.vm.network "public_network"
      machine.vm.provider "virtualbox" do |vb|
        vb.name = "#{name}"
        vb.memory = conf["memory"]
        vb.cpus = conf["cpu"]
        
      end
      machine.vm.provision "shell", path: "instalar-docker.sh"   
    end
  end
end
```
