Vagrant.configure('2') do |config|
  config.ssh.forward_agent = true

  config.vm.define "app" do |node|
    node.vm.box = "tweedegolf/debian-jessie"
    node.vm.synced_folder Dir.home, mount_homedir_location, type: "nfs", mount_options: mount_options

    node.vm.network :private_network, type: "dhcp"
    node.vm.network :forwarded_port, guest: 2375, host: 2375 # Docker
    node.vm.network :forwarded_port, guest: 80, host: 8880 # Production HTTP
    node.vm.network :forwarded_port, guest: 8080, host: 8080 # Development HTTP
    node.vm.network :forwarded_port, guest: 1080, host: 1080 # Mailhog / Misc. HTTP
    node.vm.network :forwarded_port, guest: 2080, host: 2080 # Sphinx / Misc. HTTP
    node.vm.network :forwarded_port, guest: 3080, host: 3080 # Misc HTTP
    node.vm.network :forwarded_port, guest: 3306, host: 3306 # MySQL
    node.vm.network :forwarded_port, guest: 4000, host: 4000 # Sometimes used for jekyll
    node.vm.network :forwarded_port, guest: 4080, host: 4080 # Misc HTTP
    node.vm.network :forwarded_port, guest: 4444, host: 4444 # Selenium
    node.vm.network :forwarded_port, guest: 5080, host: 5080 # Misc HTTP
    node.vm.network :forwarded_port, guest: 5432, host: 5432 # PostgreSQL
    node.vm.network :forwarded_port, guest: 6080, host: 6080 # Misc HTTP
    node.vm.network :forwarded_port, guest: 6379, host: 6379 # Redis
    node.vm.network :forwarded_port, guest: 9200, host: 9200 # Elasticsearch
    node.vm.network :forwarded_port, guest: 9300, host: 9300 # Elasticsearch
    node.vm.network :forwarded_port, guest: 15900, host: 15900 # VNC browser (selenium)
    node.vm.network :forwarded_port, guest: 15901, host: 15901 # VNC browser (selenium)
    node.vm.network :forwarded_port, guest: 35729, host: 35729 # Livereload

    # Setup large machine with two cores and 4GB memory
    node.vm.provider "virtualbox" do |virtualbox|
      virtualbox.cpus = 2
      virtualbox.memory = 4096
      virtualbox.customize ['modifyvm', :id, '--cableconnected1', 'on']
    end

    # Install docker
    node.vm.provision :docker

    # Set docker to expose itself to the host machine
    node.vm.provision :shell, inline: "
      mkdir -p /etc/systemd/system/docker.service.d &&
      printf \"[Service]\\nExecStart=\\nExecStart=/usr/bin/docker daemon -H fd:// -H tcp://0.0.0.0:2375 $OPTIONS\\n\" > /etc/systemd/system/docker.service.d/listen_address.conf &&
      systemctl daemon-reload &&
      systemctl restart docker
    "
  end
end

def is_windows
  return (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
end

def mount_options
  if is_windows
    return []
  else
    return ["nolock", "vers=3", "udp", "noatime", "actimeo=1"]
  end
end

def mount_homedir_location
  if is_windows
    Dir.home.gsub(":\\\\", "/").gsub("\\", "/")
  else
    Dir.home
  end
end
