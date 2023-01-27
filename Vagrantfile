Vagrant.configure("2") do |config|
	(1..4).each do |i|
		config.vm.define "server#{i}" do |web|
			web.vm.box = "jeffnoxon/ubuntu-20.04-arm64"
			web.vm.box_version = "1.0.0"
			web.vm.network "forwarded_port", host: 3000 + i, guest: 22#, auto_correct: true
			web.vm.network "private_network", ip: "10.11.10.#{i}"
			web.vm.hostname = "server#{i}"

			web.vm.provision "shell" do |s|
				ssh_pub_key = File.readlines("/Users/peri/.ssh/id_rsa.pub").first.strip
				s.inline = <<-SHELL
				echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
				echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
				SHELL
			end

			web.vm.provider "parallels" do |v|
				v.name = "server#{i}"
				v.memory = 2048
				v.cpus = 1
			end
		end
	end
end
