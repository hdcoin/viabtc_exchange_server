# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.box = "bento/ubuntu-16.04"

  VM_HOST = "10.50.1.1"
  MYSQL_HOST = "10.50.1.2"
  REDIS_HOST = "10.50.1.3"
  KAFKA_HOST = "10.50.1.4"
  MATCH_HOST = "10.50.1.4"

  MYSQL_USER = "viaxch"
  MYSQL_PASS = "not_production"
  REDIS_PASS = nil

  config.vm.define "mysql" do |mysql|
      mysql.vm.network "private_network", ip: MYSQL_HOST
      mysql.vm.provision :ansible do |ansible|
        ansible.playbook = "provisioning/mysql.yml"
        ansible.extra_vars = { root_dir: "/vagrant", mysql_user: MYSQL_USER, mysql_pass: MYSQL_PASS, mysql_user_host: MATCH_HOST, admin_host: VM_HOST }
      end
  end

  config.vm.define "redis" do |redis|
      redis.vm.network "private_network", ip: REDIS_HOST
      redis.vm.provision :ansible do |ansible|
        ansible.playbook = "provisioning/redis.yml"
        ansible.extra_vars = { root_dir: "/vagrant", redis_pass: REDIS_PASS, redis_host: REDIS_HOST}
      end
  end

  config.vm.define "kafka" do |kafka|
      kafka.vm.provision :ansible do |ansible|
        ansible.playbook = "provisioning/kafka.yml"
        ansible.extra_vars = { root_dir: "/vagrant" }
      end
  end

  config.vm.define "match_svr" do |match_svr|
      match_svr.vm.network "private_network", ip: MATCH_HOST
      match_svr.vm.provision :ansible do |ansible|
        ansible.playbook = "provisioning/match_svr.yml"
        ansible.extra_vars = { root_dir: "/vagrant", mysql_user: MYSQL_USER, mysql_pass: MYSQL_PASS, mysql_host: MYSQL_HOST, kafka_host: KAFKA_HOST }
      end
  end

  config.vm.define "price_svr" do |price_svr|
      price_svr.vm.network "private_network", ip: MATCH_HOST
      price_svr.vm.provision :ansible do |ansible|
        ansible.playbook = "provisioning/price_svr.yml"
        ansible.extra_vars = { root_dir: "/vagrant", redis_host: REDIS_HOST, kafka_host: KAFKA_HOST }
      end
  end
end
