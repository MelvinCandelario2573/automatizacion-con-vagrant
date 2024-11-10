Vagrant.configure("2") do |config|
    # Configuración general
    config.vm.box = "ubuntu/bionic64"
    config.vm.network "private_network", type: "dhcp"
  
    # Servidor Web 1 (Apache)
    config.vm.define "web1" do |web1|
      web1.vm.hostname = "web1"
      web1.vm.network "private_network", type: "static", ip: "192.168.33.10"
      web1.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      web1.vm.provision "shell", inline: <<-SHELL
        sudo apt update
        sudo apt install -y apache2
        sudo systemctl enable apache2
        sudo systemctl start apache2
        echo "Hello from Web 1!" | sudo tee /var/www/html/index.html
      SHELL
    end
  
    # Servidor Web 2 (Apache)
    config.vm.define "web2" do |web2|
      web2.vm.hostname = "web2"
      web2.vm.network "private_network", type: "static", ip: "192.168.33.11"
      web2.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      web2.vm.provision "shell", inline: <<-SHELL
        sudo apt update
        sudo apt install -y apache2
        sudo systemctl enable apache2
        sudo systemctl start apache2
        echo "Hello from Web 2!" | sudo tee /var/www/html/index.html
      SHELL
    end
  
    # Servidor Balanceador (Nginx)
    config.vm.define "load_balancer" do |lb|
      lb.vm.hostname = "load_balancer"
      lb.vm.network "private_network", type: "static", ip: "192.168.33.12"
      lb.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      lb.vm.provision "shell", inline: <<-SHELL
        sudo apt update
        sudo apt install -y nginx
        sudo systemctl enable nginx
        sudo systemctl start nginx
        # Configurar Nginx para hacer load balancing
        echo 'upstream backend {
          server 192.168.33.10;
          server 192.168.33.11;
        }
  
        server {
          listen 80;
  
          location / {
            proxy_pass http://backend;
          }
        }' | sudo tee /etc/nginx/sites-available/default
        sudo systemctl restart nginx
      SHELL
    end
  end
  