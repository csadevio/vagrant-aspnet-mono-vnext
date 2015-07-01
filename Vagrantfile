# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "forwarded_port", guest: 5000, host: 5000

	# ref https://github.com/aspnet/Home/blob/dev/GettingStartedDeb.md

$script = <<SCRIPT

sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get install mono-complete -y
sudo apt-get install vim -y
sudo apt-get install automake -y
sudo apt-get install libtool -y
sudo apt-get install curl -y
sudo apt-get install unzip -y
sudo apt-get update

sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
echo "deb http://download.mono-project.com/repo/debian wheezy main" | sudo tee /etc/apt/sources.list.d/mono-xamarin.list
sudo apt-get update

curl -sSL https://github.com/libuv/libuv/archive/v1.4.2.tar.gz | sudo tar zxfv - -C /usr/local/src
cd /usr/local/src/libuv-1.4.2
sudo sh autogen.sh
sudo ./configure
sudo make 
sudo make install
sudo rm -rf /usr/local/src/libuv-1.4.2 && cd ~/
sudo ldconfig

curl -sSL https://raw.githubusercontent.com/aspnet/Home/dev/dnvminstall.sh | DNX_BRANCH=dev sh && source ~/.dnx/dnvm/dnvm.sh
sudo chown vagrant.vagrant -R ~/.dnx
dnvm update-self
dnvm upgrade

mkdir ~/.config/NuGet -p
echo "<?xml version=\"1.0\" encoding=\"utf-8\"?>" >  ~/.config/NuGet/NuGet.config
echo "<configuration>" >>  ~/.config/NuGet/NuGet.config
echo "		<packageSources>" >>  ~/.config/NuGet/NuGet.config
echo "			<add key=\"AspNetVNext\" value=\"https://www.myget.org/F/aspnetvnext/api/v2\" />" >>  ~/.config/NuGet/NuGet.config
echo "			<add key=\"nuget.org\" value=\"https://www.nuget.org/api/v2/\" />" >>  ~/.config/NuGet/NuGet.config
echo "		</packageSources>" >>  ~/.config/NuGet/NuGet.config
echo "	<disabledPackageSources />" >>  ~/.config/NuGet/NuGet.config
echo "</configuration>" >>  ~/.config/NuGet/NuGet.config

sudo apt-get install nuget -y

SCRIPT

  config.vm.provision "shell", inline: $script, privileged: false

end
