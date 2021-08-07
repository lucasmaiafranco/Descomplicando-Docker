Vagrant.configure("2") do |config|

  config.vm.define "docker" do |docker|
    docker.vm.box = "debian/buster64"
	docker.vm.hostname = "docker-server"
	docker.vm.network "forwarded_port", guest: 80, host: 8000
	docker.vm.network "forwarded_port", guest: 8080, host: 8080
  end
end