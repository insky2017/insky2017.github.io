
## Build latest vim
``` bash
#!/usr/bin/env bash
# sudo apt update
# sudo apt install -y git
# sudo apt install -y build-essential
rm -fr /tmp/vim
git clone https://github.com/vim/vim.git /tmp/vim
make -C /tmp/vim
sudo make install -C /tmp/vim 
rm -fr /tmp/vim
```

---


## Failed to bring up SpaceVim
1.  change the default spacevim_plugin_managerin autoload/SpaceVim.vim to neobundle like:
``` bash
sed -i "s/'dein'/'neobundle'/" ~/.SpaceVim/autoload/SpaceVim.vim
```
2. install vimproc like:
``` bash
apt-get install build-essential make
git clone https://github.com/Shougo/vimproc.vim; cd vimproc.vim; make
Inside that very directory open vim and inside vim type
:VimProcInstall
```

---


## Display CRLF as ^M:
```
:e ++ff=unix
```

## Substitute CRLF for LF:
```
:setlocal ff=unix  
:w  
:e  
```

