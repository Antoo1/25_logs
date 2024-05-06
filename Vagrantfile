MACHINES = {
  :"web" => {
              :box_name => "generic/ubuntu2204",
              :cpus => 1,
              :memory => 512,
              :ip => "192.168.56.222",
            },
  :"log" => {
              :box_name => "generic/ubuntu2204",
              :cpus => 1,
              :memory => 2048,
              :ip => "192.168.56.223",
            },
  :"extravm" => {
              :box_name => "generic/ubuntu2204",
              :cpus => 1,
              :memory => 512,
              :ip => "192.168.56.224",
            }
}

Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
    config.vm.synced_folder ".", "/vagrant", disabled: true
    config.vm.define boxname do |box|
      box.vm.network "private_network", ip: boxconfig[:ip]
      box.vm.host_name = boxname
      box.vm.box = boxconfig[:box_name]
      box.vm.box_version = boxconfig[:box_version]
      box.vm.host_name = boxname.to_s

      box.vm.provider "libvrt" do |v|
        v.memory = boxconfig[:memory]
        v.cpus = boxconfig[:cpus]
      end
      box.vm.provision "shell", inline: <<-SHELL
          sed -i 's/^PasswordAuthentication.*$/PasswordAuthentication no/' $(grep -r 'PasswordAuthentication' /etc/ssh/ | awk '{print $1}' FS=':' | uniq)
          systemctl restart sshd.service
  	  SHELL
    end
  end
end

