# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"

   config.vm.provider "virtualbox" do |vb|
     # Display the VirtualBox GUI when booting the machine
     vb.gui = true

     #Enable audio (different drivers are required depending on host OS)
     if RUBY_PLATFORM =~ /darwin/
       vb.customize ["modifyvm", :id, '--audio', 'coreaudio', '--audiocontroller', 'hda']
     elsif RUBY_PLATFORM =~ /mingw|mswin|bccwin|cygwin|emx/
       vb.customize ["modifyvm", :id, '--audio', 'dsound', '--audiocontroller', 'ac97']
     elsif RUBY_PLATFORM =~ /linux/
       vb.customize ["modifyvm", :id, "--audio", "pulse", "--audiocontroller", "hda"]
     end

     #Enable video acceleration and increase the VRAM
     vb.customize ["modifyvm", :id, "--accelerate3d", "on"]
     vb.customize ["modifyvm", :id, "--accelerate2dvideo", "on"]
     vb.customize ["modifyvm", :id, "--vram", "128"]

     # Customize the amount of memory on the VM:
     vb.memory = "1024"
   end

   #Provision the VM
   config.vm.provision "shell", inline: <<-SHELL
     export DEBIAN_FRONTEND=noninteractive
     apt-get update
     #Install lightdm , lxde & the virtualbox video driver
     apt install -y --force-yes lxde-core lxtask lxrandr lxterminal xserver-xorg-video-all virtualbox-guest-x11 virtualbox-guest-dkms pulseaudio lightdm
     #Setup autologin
     sudo usermod -a -G nopasswdlogin ubuntu 
     sudo echo "[Seat:*]" >> /usr/share/lightdm/lightdm.conf.d/50-autologin.conf
     sudo echo "autologin-user=ubuntu" >> /usr/share/lightdm/lightdm.conf.d/50-autologin.conf
     sudo echo "autologin-user-timeout=0" >> /usr/share/lightdm/lightdm.conf.d/50-autologin.conf 
     sudo dpkg-reconfigure lightdm 
     #Clean up the drive 
     sudo apt-get clean
     cat /dev/null > ~/.bash_history && history -c
     sudo dd if=/dev/zero of=/EMPTY bs=1M || true
     sudo rm -f /EMPTY
   SHELL
end
