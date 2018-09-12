
Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/xenial64"
  config.vm.network "forwarded_port", guest: 3838, host: 3838
  config.vm.network "forwarded_port", guest: 8787, host: 8787
  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--memory", "4096"]
    vb.customize ["modifyvm", :id, "--cpus", "2"]
  end

$script = <<BOOTSTRAP
sudo apt-get update
sudo apt-get -y install git build-essential gfortran autoconf automake mc
sudo apt-get -y install libxml2-dev libcurl4-openssl-dev libssl0.9.8 libcairo2-dev
sudo add-apt-repository ppa:marutter/rrutter
sudo apt-get update
sudo apt-get -y install r-base r-base-dev
sudo R -e "install.packages('shiny', repos = 'http://cran.rstudio.com/', dep = TRUE)"
sudo R -e "install.packages('rmarkdown', repos = 'http://cran.rstudio.com/', dep = TRUE)"
sudo apt-get -y install gdebi-core
wget https://download3.rstudio.org/ubuntu-14.04/x86_64/shiny-server-1.5.8.921-amd64.deb
sudo gdebi shiny-server-1.2.3.368-amd64.deb
sudo dpkg -i *.deb
rm *.deb
wget https://download2.rstudio.org/rstudio-server-1.1.456-amd64.deb
sudo dpkg -i *.deb
rm *.deb
sudo ln -s /vagrant/apps /srv/shiny-server
sudo usermod -a -G vagrant shiny
BOOTSTRAP

  config.vm.provision :shell, :inline => $script
end
