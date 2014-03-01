host_cache_path = File.expand_path("../.cache", __FILE__)
guest_cache_path = "/tmp/vagrant-cache"

# ensure the cache path exists
FileUtils.mkdir(host_cache_path) unless File.exist?(host_cache_path)

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  
  config.vm.define :jenkins do |config| 

    config.berkshelf.enabled = true

    config.vm.provider :virtualbox do |vb|      
      vb.gui = true
    end

    config.vm.box = "precise64"
    config.vm.network :private_network, :ip => "10.1.2.11"
    config.vm.synced_folder host_cache_path, guest_cache_path
    config.vm.hostname = "jenkins"
    
    config.vm.provision :shell, :inline => "echo '10.1.2.10 chef' >> /etc/hosts"

    config.vm.provision "chef_client" do |chef_config|
      chef_config.chef_server_url = "https://chef"
      chef_config.validation_key_path = "/Users/ieu94897/.chef/chef-validator.pem"

      chef_config.add_recipe "jenkins"
    end
  end
end
