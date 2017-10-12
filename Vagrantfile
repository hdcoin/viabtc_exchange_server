# -*- mode: ruby -*-
# vi: set ft=ruby :

combine_servers = true

Vagrant.configure(2) do |config|

  config.vm.box = "bento/ubuntu-16.04"

  if combine_servers
    config.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
      v.memory = 2048
      v.cpus = 2
    end
  end

  VM_HOST    = "10.50.1.1"
  if combine_servers
      COMBINE_HOST = MYSQL_HOST = REDIS_HOST = KAFKA_HOST = MATCH_HOST = PRICE_HOST = DATA_HOST = HTTP_HOST = WS_HOST = "10.50.1.2"
  else
      MYSQL_HOST = "10.50.1.2"
      REDIS_HOST = "10.50.1.3"
      KAFKA_HOST = "10.50.1.4"
      MATCH_HOST = "10.50.1.5"
      PRICE_HOST = "10.50.1.6"
      DATA_HOST  = "10.50.1.7"
      HTTP_HOST  = "10.50.1.8"
      WS_HOST    = "10.50.1.9"
  end

  MYSQL_USER = "viaxch"
  MYSQL_PASS = "not_production"
  REDIS_PASS = nil

  #TODO: frontend endpoint to validate can stream user events?
  AUTH_URL = "http://10.50.1.10:8000/internal/exchange/user/auth"

  mysql_vars = { root_dir: "/vagrant", mysql_user: MYSQL_USER, mysql_pass: MYSQL_PASS, mysql_user_match_host: MATCH_HOST, mysql_user_data_host: DATA_HOST, admin_host: VM_HOST }
  redis_vars = { root_dir: "/vagrant", redis_pass: REDIS_PASS, redis_host: REDIS_HOST}
  kafka_vars = { root_dir: "/vagrant" }
  match_vars = { root_dir: "/vagrant", mysql_user: MYSQL_USER, mysql_pass: MYSQL_PASS, mysql_host: MYSQL_HOST, kafka_host: KAFKA_HOST }
  price_vars = { root_dir: "/vagrant", redis_host: REDIS_HOST, kafka_host: KAFKA_HOST }
  data_vars =  { root_dir: "/vagrant", mysql_user: MYSQL_USER, mysql_pass: MYSQL_PASS, mysql_host: MYSQL_HOST }
  http_vars =  { root_dir: "/vagrant", match_host: MATCH_HOST, price_host: PRICE_HOST, data_host: DATA_HOST }
  ws_vars =    { root_dir: "/vagrant", match_host: MATCH_HOST, price_host: PRICE_HOST, data_host: DATA_HOST, kafka_host: KAFKA_HOST, auth_url: AUTH_URL }

  if combine_servers
      config.vm.define "main_svr" do |main_svr|
          main_svr.vm.network "private_network", ip: COMBINE_HOST
          main_svr.vm.provision "mysql", type: "ansible" do |ansible|
            ansible.playbook = "provisioning/mysql.yml"
            ansible.extra_vars = mysql_vars
          end
          main_svr.vm.provision "redis", type: "ansible" do |ansible|
            ansible.playbook = "provisioning/redis.yml"
            ansible.extra_vars = redis_vars
          end
          main_svr.vm.provision "kafka", type: "ansible" do |ansible|
            ansible.playbook = "provisioning/kafka.yml"
            ansible.extra_vars = kafka_vars
          end
          main_svr.vm.provision "match_svr", type: "ansible" do |ansible|
            ansible.playbook = "provisioning/match_svr.yml"
            ansible.extra_vars = match_vars
          end
          main_svr.vm.provision "price_svr", type: "ansible" do |ansible|
            ansible.playbook = "provisioning/price_svr.yml"
            ansible.extra_vars = price_vars
          end
          main_svr.vm.provision "data_svr", type: "ansible" do |ansible|
            ansible.playbook = "provisioning/data_svr.yml"
            ansible.extra_vars = data_vars
          end
          main_svr.vm.provision "http_svr", type: "ansible" do |ansible|
            ansible.playbook = "provisioning/http_svr.yml"
            ansible.extra_vars = http_vars
          end
          main_svr.vm.provision "ws_svr", type: "ansible" do |ansible|
            ansible.playbook = "provisioning/ws_svr.yml"
            ansible.extra_vars = ws_vars
          end
      end
  else
      config.vm.define "mysql" do |mysql|
          mysql.vm.network "private_network", ip: MYSQL_HOST
          mysql.vm.provision :ansible do |ansible|
            ansible.playbook = "provisioning/mysql.yml"
            ansible.extra_vars = mysql_vars
          end
      end

      config.vm.define "redis" do |redis|
          redis.vm.network "private_network", ip: REDIS_HOST
          redis.vm.provision :ansible do |ansible|
            ansible.playbook = "provisioning/redis.yml"
            ansible.extra_vars = redis_vars
          end
      end

      config.vm.define "kafka" do |kafka|
          kafka.vm.network "private_network", ip: KAFKA_HOST
          kafka.vm.provision :ansible do |ansible|
            ansible.playbook = "provisioning/kafka.yml"
            ansible.extra_vars = kafka_vars
          end
      end

      config.vm.define "match_svr" do |match_svr|
          match_svr.vm.network "private_network", ip: MATCH_HOST
          match_svr.vm.provision :ansible do |ansible|
            ansible.playbook = "provisioning/match_svr.yml"
            ansible.extra_vars = match_vars
          end
      end

      config.vm.define "price_svr" do |price_svr|
          price_svr.vm.network "private_network", ip: PRICE_HOST
          price_svr.vm.provision :ansible do |ansible|
            ansible.playbook = "provisioning/price_svr.yml"
            ansible.extra_vars = price_vars
          end
      end

      config.vm.define "data_svr" do |data_svr|
          data_svr.vm.network "private_network", ip: DATA_HOST
          data_svr.vm.provision :ansible do |ansible|
            ansible.playbook = "provisioning/data_svr.yml"
            ansible.extra_vars = data_vars
          end
      end

      config.vm.define "http_svr" do |http_svr|
          http_svr.vm.network "private_network", ip: HTTP_HOST
          http_svr.vm.provision :ansible do |ansible|
            ansible.playbook = "provisioning/http_svr.yml"
            ansible.extra_vars = http_vars
          end
      end

      config.vm.define "ws_svr" do |ws_svr|
          ws_svr.vm.network "private_network", ip: WS_HOST
          ws_svr.vm.provision :ansible do |ansible|
            ansible.playbook = "provisioning/ws_svr.yml"
            ansible.extra_vars = ws_vars
          end
      end
  end
end
