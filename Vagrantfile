Vagrant.configure("2") do |config|
   
    config.vm.define "web1" do |web1|
      web1.vm.box = "ubuntu/jammy64" 
      web1.vm.network "private_network", ip: "192.168.56.4" 
      web1.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      web1.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y apache2
        rm -rf /var/www/html
        ln -s /vagrant/web1 /var/www/html
      SHELL
      web1.vm.synced_folder "./web1", "/vagrant/web1" 
    end
  
    
    config.vm.define "web2" do |web2|
      web2.vm.box = "ubuntu/jammy64"
      web2.vm.network "private_network", ip: "192.168.56.5" 
      web2.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      web2.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y apache2
        rm -rf /var/www/html
        ln -s /vagrant/web2 /var/www/html
      SHELL
      web2.vm.synced_folder "./web2", "/vagrant/web2" 
    end
  
    
    config.vm.define "loadbalancer" do |lb|
      lb.vm.box = "ubuntu/jammy64"
      lb.vm.network "private_network", ip: "192.168.56.6" 
      lb.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      lb.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y nginx
        
        cat <<EOF > /etc/nginx/conf.d/load_balancer.conf
        upstream backend {
            server 192.168.56.4;
            server 192.168.56.5;
        }
  
        server {
            listen 80;
  
            location / {
                proxy_pass http://backend;
            }
        }
        EOF
        systemctl restart nginx
      SHELL
    end
  end