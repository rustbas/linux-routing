# Default box
box_name = "debian.jessie64.libvirt.box"

# List of nodes
nodes = [
  { :memory => 512, :cpu => 1 },
  { :memory => 512, :cpu => 1 },
  { :memory => 512, :cpu => 1 },
]

Vagrant.configure("2") do |config|

  # Nodes configs
  nodes.each_with_index do |node, i|
    config.vm.box_check_update = false
    config.vm.define "node-#{ i+1 }" do |nodeconfig|
      nodeconfig.vm.box = box_name
      nodeconfig.vm.hostname = "host-#{ i+1 }"
      nodeconfig.vm.network :private_network, ip: "10.200.#{ i+1 }.2"
      # nodeconfig.vm.provision "shell", inline: $SET_ENVIRONMENT, run: "always"
      nodeconfig.vm.provider :libvirt do |vb|
        vb.memory = node[:memory]
        vb.cpus = node[:cpu]
      end

      # nodeconfig.vm.provision "ansible" do |ansible|
        # ansible.compatibility_mode = "2.0"
        # ansible.playbook = "provisioning/playbook.yaml"
        # ansible.become = true
      # end
      # config.vm.provision "ansible" do |ansible|
        # ansible.playbook = "provisioning/playbook.yaml"
        # ansible.groups = {
          # "elk"   => ["node-1"],
        # }
      # end
    end
  end
end
