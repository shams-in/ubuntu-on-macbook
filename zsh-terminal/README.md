```
$ sudo apt update
$ sudo apt install zsh

# verify
$ zsh --version

# change default shell
$ echo $SHELL
$ chsh -s /usr/bin/zsh

# restart ubuntu

# install oh my zsh
$ sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

# change theme

    $ nano ~/.zshrc

        ZSH_THEME="agnoster"

    # install fonts for above theme
    $ sudo apt-get install fonts-powerline

    # reload
    $ source ~/.zshrc

# add plugins

    $ git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH/plugins/zsh-autosuggestions

    $ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH/plugins/zsh-syntax-highlighting

    $ ls $ZSH/plugins | grep zsh-

    $ nano ~/.zshrc

        plugins=(... zsh-autosuggestions zsh-syntax-highlighting)

    $ source ~/.zshrc
