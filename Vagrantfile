# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'rubygems'
require 'fileutils'
require 'yaml'

docker_ee_repo = ENV['DOCKER_EE_REPO']

provision_hosts_ee = <<SCRIPT
rpm --import "https://sks-keyservers.net/pks/lookup?op=get&search=0xee6d536cf7dc86e2d7d56f59a178ac6c6238f52e"
cat <<EOF > /etc/yum/vars/dockerurl
$2
EOF
yum install -y yum-utils
yum-config-manager \
    --add-repo \
        $2/docker-ee.repo
yum makecache fast
yum -y install docker-ee
systemctl enable docker
systemctl start docker
systemctl -q is-active firewalld && systemctl stop firewalld
systemctl -q is-enabled firewalld && systemctl disable firewalld
usermod -a -G docker vagrant
cat <<EOF > /etc/hosts
$1
EOF
SCRIPT

provision_hosts_ce = <<SCRIPT
rpm --import "https://sks-keyservers.net/pks/lookup?op=get&search=0xee6d536cf7dc86e2d7d56f59a178ac6c6238f52e"
yum install -y yum-utils
yum-config-manager \
    --add-repo \
        https://download.docker.com/linux/centos/docker-ce.repo
yum makecache fast
yum -y install docker-ce
systemctl enable docker
systemctl start docker
systemctl -q is-active firewalld && systemctl stop firewalld
systemctl -q is-enabled firewalld && systemctl disable firewalld
usermod -a -G docker vagrant
cat <<EOF > /etc/hosts
$1
EOF
SCRIPT

provision_master = <<SCRIPT
docker pull docker/ucp:2.1.1
docker run --rm --name ucp \
 -v /var/run/docker.sock:/var/run/docker.sock \
 docker/ucp:2.1.1 install \
 --host-address $1 \
 --admin-username admin --admin-password AdminPassword --san $1
docker swarm join-token manager | \
  grep -A 20 "docker swarm join" > /shared/join_manager.sh
docker swarm join-token worker | \
  grep -A 20 "docker swarm join" > /shared/join_worker.sh
SCRIPT

provision_dtr = <<SCRIPT
docker run --rm \
  docker/dtr:2.2.3 install \
    --ucp-node $1 \
        --ucp-insecure-tls
SCRIPT

provision_worker = <<SCRIPT
chmod +x /shared/join_worker.sh
/shared/join_worker.sh
SCRIPT

CENTOS = 'centos'.freeze
num_nodes = 2
start = 50
base_ip = '192.168.2.'
ucp_master = '192.168.2.' + start.to_s
node_ips = Array.new(num_nodes) { |n| base_ip + (n + start).to_s }
node_names = Array.new(num_nodes) { |n| "ucp-node#{n}" }
hosts = "127.0.0.1   localhost\n"
hosts << "#{base_ip}0   netmaster\n"
hosts = (0..num_nodes).inject(hosts) { |acc, elem| acc << base_ip + "#{elem} ucp-node#{elem} \n" }

VAGRANTFILE_API_VERSION = '2'.freeze
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box_check_update = false
  config.vm.synced_folder './export', '/shared'

  num_nodes.times do |n|
    node_name = node_names[n]
    node_addr = node_ips[n]
    config.vm.define vm_name = node_name do |c|
      c.vm.box = 'centos/7'
      config.ssh.insert_key = false
      # configure ip address etc
      c.vm.hostname = vm_name
      c.vm.network :private_network, ip: node_addr
      c.vm.network :private_network, ip: node_addr, virtualbox__intnet: 'true', auto_config: false
      if docker_ee_repo
        c.vm.provision :shell, inline: provision_hosts_ee, args: [hosts, docker_ee_repo]
      else
        c.vm.provision :shell, inline: provision_hosts_ce, args: [hosts]
      end
      if n.zero?
        c.vm.provider 'virtualbox' do |v|
          v.memory = 4096
        end # v
        c.vm.provision :shell, inline: provision_master, args: [ucp_master]
        c.vm.provision :shell, inline: 'docker pull docker/ucp:2.1.1'
      else
        c.vm.provision :shell, inline: provision_worker
      end # if
    end # c
  end # role
end # config
