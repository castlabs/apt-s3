Vagrant.configure("2") do |config|
  config.vm.box       = "precise64"
  config.vm.box_url   = "http://files.vagrantup.com/precise64.box"

  config.vm.network :public_network

  config.vm.provision :shell, :inline => "apt-get update"
  config.vm.provision :shell, :inline => "apt-get install -y libapt-pkg-dev libcurl4-openssl-dev debhelper cdbs"
  config.vm.provision :shell, :inline => "apt-get install -y build-essential"
  config.vm.provision :shell, :inline => "cp -r /vagrant apt-s3 ; (cd apt-s3 ; make ; make deb)"
  config.vm.provision :shell, :inline => "cp *.deb /vagrant/debian"

  config.vm.provider :virtualbox do |vb|
  end
end
