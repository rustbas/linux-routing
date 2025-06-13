# Default box
box_name = "debian.bookworm.libvirt.box"

# List of nodes
nodes = [
  { :memory => 1024, :cpu => 1 },
  { :memory => 1024, :cpu => 1 },
]

router = { :memory => 512, :cpu => 1 }


Vagrant.configure("2") do |config|

  # Hosts configuration
  nodes.each_with_index do |node, i|
    config.vm.box_check_update = false

    config.vm.define "host-#{ i+1 }" do |nodeconfig|
      nodeconfig.vm.box      = box_name
      nodeconfig.vm.hostname = "host-#{ i+1 }"

      nodeconfig.vm.network :private_network, ip: "10.0.#{ i+1 }.10" , virtualbox__intnet: "subnet#{i+1}"

      nodeconfig.vm.provider :libvirt do |vb|
        vb.memory = node[:memory]
        vb.cpus   = node[:cpu]
      end
      nodeconfig.vm.provision "shell", inline: <<-SHELL
        ip route replace default via 10.0.#{ i+1 }.1
        # echo "nameserver 8.8.8.8" | tee /etc/resolv.conf
      SHELL
    end



  end

  # Router configuration
  config.vm.define "router" do |nodeconfig|
    nodeconfig.vm.box      = box_name
    nodeconfig.vm.hostname = "router"

    nodeconfig.vm.network :private_network, ip: "10.0.1.1", virtualbox__intnet: "subnet1"
    nodeconfig.vm.network :private_network, ip: "10.0.2.1", virtualbox__intnet: "subnet2"

    nodeconfig.vm.provider :libvirt do |vb|
      vb.memory = router[:memory]
      vb.cpus   = router[:cpu]
    end

    # Manual ping to create ARP-requests
    nodeconfig.vm.provision "shell", inline: <<-SHELL
      # ARP-records lifetime
      sysctl -w net.ipv4.neigh.default.gc_stale_time=600

      # Simplier 
      sysctl -w net.ipv4.neigh.default.gc_thresh1=1024
      sysctl -w net.ipv4.neigh.default.gc_thresh2=2048
      sysctl -w net.ipv4.neigh.default.gc_thresh3=4096

      # ping -W 10 -c 5 10.0.1.10
      # ping -W 10 -c 5 10.0.2.10
    SHELL
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/playbook.yaml"
    ansible.groups = {
      "pc"      => ["host-1", "host-2"],
      "routers" => ["router"],
    }
  end

end
