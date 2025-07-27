Vagrant.configure("2") do |config|
  # Use Ubuntu 22.04 base image from Bento
  config.vm.box = "bento/ubuntu-22.04"

  # Configure VM resources (adjust as needed)
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"  # Allocate 4GB RAM
    vb.cpus = 2         # Use 2 CPU cores
  end

  # Optional: Set a private static IP (good for multiple VMs)
  # config.vm.network "private_network", ip: "10.0.2.100"
  config.vm.network "public_network"

  # Extend boot timeout to avoid provisioning issues on slower machines
  config.vm.boot_timeout = 600  # 10 minutes

  # Read .env variables (you must have the 'dotenv' gem installed on host)
  require 'dotenv'
  Dotenv.load('.env')

  # Shell provisioner to install Docker and login securely
  config.vm.provision "shell", inline: <<-SHELL
    echo "******************** / system update / ********************"
    sudo apt-get update -y

    echo "******************** / system upgrade / ********************"
    sudo apt-get upgrade -y

    echo "******************** / Install required packages for Docker setup / ********************"
    sudo apt-get install -y python3 apt-transport-https ca-certificates curl software-properties-common gnupg lsb-release

    echo "******************** / Add Docker’s GPG key and set up repository / ********************"
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
      sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
      https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

    echo "******************** / Install Docker Engine and Docker Compose plugin / ********************"
    sudo apt-get update -y
    sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

    echo "******************** / Enable and start Docker / ********************"
    sudo systemctl enable docker
    sudo systemctl start docker

    echo "******************** / Add vagrant user to Docker group / ********************"
    sudo usermod -aG docker vagrant

    echo "******************** / Display Docker versions / ********************"
    docker --version
    docker compose version || echo "Docker Compose plugin installed"

    echo "******************** / Logging in to Docker Hub / ********************"
    if [ -f "/vagrant/.env" ]; then
      export $(grep DOCKER_USERNAME /vagrant/.env)
      export $(grep DOCKER_PASSWORD /vagrant/.env)
      echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
    else
      echo ".env file not found. Skipping Docker login."
    fi
  SHELL
end
