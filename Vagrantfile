Vagrant.configure("2") do |config|
    # Configuración del servidor Apache 1
    config.vm.define "apache1" do |apache1|
      apache1.vm.box = "ubuntu/bionic64"
      apache1.vm.network "private_network", ip: "192.168.56.11"
      apache1.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      apache1.vm.synced_folder "./web1", "/var/www/html"
      apache1.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
        sudo systemctl enable apache2
        echo "<h1>Hola desde Apache Server 1</h1>" > /var/www/html/index.html
      SHELL
    end
  
    # Configuración del servidor Apache 2
    config.vm.define "apache2" do |apache2|
      apache2.vm.box = "ubuntu/bionic64"
      apache2.vm.network "private_network", ip: "192.168.56.12"
      apache2.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      apache2.vm.synced_folder "./web2", "/var/www/html"
      apache2.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
        sudo systemctl enable apache2
        echo "<h1>Hola desde Apache Server 2</h1>" > /var/www/html/index.html
      SHELL
    end
  
    # Configuración del servidor Nginx (Balanceador de carga)
    config.vm.define "nginx" do |nginx|
      nginx.vm.box = "ubuntu/bionic64"
      nginx.vm.network "private_network", ip: "192.168.56.13"
      nginx.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      nginx.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y nginx
        sudo systemctl enable nginx
  
        # Configurar Nginx como balanceador de carga
        sudo bash -c 'cat > /etc/nginx/conf.d/load_balancer.conf << EOF
  upstream apache_servers {
      server 192.168.56.11;
      server 192.168.56.12;
  }
  
  server {
      listen 80;
  
      location / {
          proxy_pass http://apache_servers;
      }
  }
  EOF'
        sudo systemctl restart nginx
      SHELL
    end
  end