# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.box = "bento/ubuntu-16.04"

  MYSQL_USER = "viaxch"
  MYSQL_PASS = "not_production"
  MYSQL_DB = "viaxch"
  REDIS_PASS = "not_production"

  config.vm.define "mysql" do |mysql|
      mysql.vm.provision :ansible do |ansible|
        ansible.playbook = "provisioning/mysql.yml"
        ansible.extra_vars = { root_dir: "/vagrant", mysql_user: MYSQL_USER, mysql_pass: MYSQL_PASS, mysql_db: MYSQL_DB }
      end
  end

  config.vm.define "redis" do |redis|
      redis.vm.provision :ansible do |ansible|
        ansible.playbook = "provisioning/redis.yml"
        ansible.extra_vars = { root_dir: "/vagrant", redis_pass: REDIS_PASS}
      end
  end

  config.vm.define "kafka" do |kafka|
      kafka.vm.provision :ansible do |ansible|
        ansible.playbook = "provisioning/kafka.yml"
        ansible.extra_vars = { root_dir: "/vagrant" }
      end
  end

  config.vm.define "blah" do |blah|
    blah.vm.provision :ansible do |ansible|
      ansible.playbook = "provisioning/playbook.yml"
      ansible.extra_vars = { root_dir: "/vagrant" }
    end
  end
end
