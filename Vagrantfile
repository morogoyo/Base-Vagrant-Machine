Vagrant.configure("2") do |config|
  # Use Ubuntu 22.04 base image from Bento
  config.vm.box = "bento/ubuntu-22.04"

  # Configure VM resources (adjust as needed)
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"  # Allocate 4GB RAM
    vb.cpus = 2         # Use 2 CPU cores
  end

  # Optional: Set a private static IP (good for multiple VMs)
  config.vm.network "private_network", ip: "192.168.56.10"

  # Install Docker and Docker Compose via shell provisioning
  config.vm.provision "shell", inline: <<-SHELL
    # System update
    sudo apt-get update -y
    sudo apt-get upgrade -y

    # Install required packages for Docker setup
    sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common gnupg lsb-release

    # Add Docker’s GPG key and set up repository
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
      sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
    https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

    # Install Docker Engine and Docker Compose plugin
    sudo apt-get update -y
    sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

    # Start Docker on boot
    sudo systemctl enable docker
    sudo systemctl start docker

    # Add vagrant user to Docker group for non-root access
    sudo usermod -aG docker vagrant

    # Display installed versions for confirmation
    docker --version
    docker compose version || echo "Docker Compose plugin installed"
  SHELL
end
