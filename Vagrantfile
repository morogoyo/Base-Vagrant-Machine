
# Load .env file manually (no gem required)
env_file = File.join(File.dirname(__FILE__), '.env')
if File.exist?(env_file)
  File.readlines(env_file).each do |line|
    next if line.strip.empty? || line.strip.start_with?('#')
    key, value = line.strip.split('=', 2)
    ENV[key] = value if key && value
  end
end



Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"

   # Use environment variable for synced folder
    host_folder = ENV['HOST_SHARED_FOLDER']
    host_folder_ansible = ENV['HOST_SHARED_FOLDER_ANSIBLE']


  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
    vb.cpus = 6
    vb.gui = true
  end

  ################## Shared folder from host (Windows) to guest (VM) #############################
#   config.vm.synced_folder host_folder, "/vagrant_data"

  config.vm.synced_folder host_folder_ansible, "/ansible"




  config.vm.boot_timeout = 900  # 15 minutes
  config.vm.network "public_network" , type: "dhcp"


  # Shell provisioner that reads .env inside the guest VM
#   config.vm.provision "shell", inline: <<-SHELL
#     echo "******************** / system update / ********************"
#     sudo apt-get update -y
#     sudo apt-get upgrade -y
#
#     echo "******************** / Install required packages for Docker setup / ********************"
#     sudo apt-get install -y python3 apt-transport-https ca-certificates curl software-properties-common gnupg lsb-release
#
#     echo "******************** / Add Docker GPG and repo / ********************"
#     curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
#
#     echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
#     https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
#     sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
#
#     echo "******************** / Install Docker Engine and Compose / ********************"
#     sudo apt-get update -y
#     sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
#
#     echo "******************** / Start and enable Docker / ********************"
#     sudo systemctl enable docker
#     sudo systemctl start docker
#
#     echo "******************** / Add vagrant user to Docker group / ********************"
#     sudo usermod -aG docker vagrant
#
#     echo "******************** / Source Docker credentials from .env / ********************"
#     if [ -f /vagrant/.env ]; then
#       export $(grep DOCKER_USERNAME /vagrant/.env | xargs)
#       export $(grep DOCKER_PASSWORD /vagrant/.env | xargs)
#       echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
#     else
#       echo "/vagrant/.env not found. Skipping Docker login."
#     fi
#
#     echo "******************** / Docker Info / ********************"
#     docker --version
#     docker compose version || echo "Docker Compose plugin installed"
#
# echo "******************** / pull Docker containers  / ********************"
#     docker pull morogoyo/ansible:dev
#     docker run -d --name ansible morogoyo/ansible:dev
#
#   SHELL
end
