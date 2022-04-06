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
        