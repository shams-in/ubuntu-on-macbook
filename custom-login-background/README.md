## works for ubuntu 20.03

    $ lsb_release -a

    $ sudo apt install libglib2.0-dev-bin

    $ sudo ./ubuntu-gdm-set-background --color \#000000

# other commands

```sh
# other commands
sudo ./ubuntu-gdm-set-background --image /home/user/backgrounds/image.jpg
sudo ./ubuntu-gdm-set-background --color \#aAbBcC
sudo ./ubuntu-gdm-set-background --gradient horizontal \#aAbBcC \#dDeEfF
sudo ./ubuntu-gdm-set-background --gradient vertical \#aAbBcC \#dDeEfF
sudo ./ubuntu-gdm-set-background --reset
./ubuntu-gdm-set-background --help
```

## change background color (via gnome-tweak-tool)

    $ sudo add-apt-repository universe 
    $ sudo apt install -y gnome-tweak-tool 

    $ gsettings set org.gnome.desktop.background picture-uri none
    $ gsettings set org.gnome.desktop.background primary-color '#000000'
    $ gsettings set org.gnome.desktop.background color-shading-type 'solid'
    
    Applications > Tweeks

        Theme.Applications: Adwaita-dark
        

## install gnome extension

    sudo apt install gnome-tweaks
    gnome-shell --version

    sudo apt install gnome-shell-extensions

    install extension using chrome extension - https://extensions.gnome.org/extension/3037/good-bye-gdm-flick/

## fix login screen resolution

```
    $ sudo rm -rf /var/lib/gdm3/.config/monitors.xml

    $ sudo cp ~/.config/monitors.xml /var/lib/gdm3/.config/
    
    $ sudo chown gdm:gdm /var/lib/gdm3/.config/monitors.xml
    (make sure the scale is 1)

    $ ls -ltra /var/lib/gdm3/.config

    $ cat /var/lib/gdm3/.config/monitors.xml

    sudoedit /etc/gdm3/custom.conf
    In the file uncomment WaylandEnable=false, and save.

    reboot the computer
```