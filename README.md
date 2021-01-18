# Ocaml-On-OSX
Run OCaml on a OSX. Tested on Big Sur

# Useful Configuration
Remap keys  
* Caps -> Control  

# Apps to make the experience better
Must have (don't use terminal. iTerm2 is significantly better)
* Brew: https://brew.sh/
* iTerm2: https://iterm2.com/

Recommended, but work to setup/configure
* Alfred: https://www.alfredapp.com/
* Divvy: https://mizage.com/divvy/ (Actually there probably is a better way to do this now?)

# Install OCaml stuff:
https://dev.realworldocaml.org/install.html

# RWO install [Tested on Big Sur)]
~~~~
$ brew update
$ brew upgrade
$ brew cleanup
$ brew install gpatch 
$ brew install opam
$ brew install libx11 # Dependency for async_graphics
$ opam init
$ opam switch # This shows compiler versions, check for something 4.10.0 or higher
$ # Uncomment the next two commands if you don't see >4.11.1 as the default version
$ # opam switch create 4.11.1
$ # opam switch 4.11.1
$ opam install core utop async yojson ppx_deriving_yojson core_extended core_bench menhir
$ opam depext conf-pkg-config.1.3
$ opam depext conf-gmp.3
$ opam depext conf-libpcre.1
$ opam install cohttp async_graphics cryptokit 
# done with realworld install
~~~~

# Sagar says install some tools
~~~~
$ opam install patdiff sexp ocamlformat merlin ocp-indent
~~~~

# Housekeeping
Ocaml Code directory structure I  like to put stuff in  
* workspaces
  * app
  * lib
  * external
  
In external I put all my opam pin’d libraries  

# Generally useful Linux stuff and other tools
~~~~
$ brew install coreutils fzf gzip fswatch jq nmap ripgrep p7zip tcpdump wget tmux xz
~~~~

# Install VIM config
~~~~
# Copy over my vim-config 
$ git clone https://github.com/SagarMomin/vim-config.git ~/.vim
~~~~

# Install opinionated bashrc and tmux configs in order to make sure everything works
~~~~
# If you don't want to overwrite your bashrc, you'll have to manually look through: ~/.vim/system-configs/setup/
$ cd ~/.vim/system-configs/ && bash smash-all-my-configs.sh && cd -
# Update inputrc to point to the one in system config
$ ln -s ~/.vim/system-configs/rcfiles/sane-defaults/inputrc ~/.inputrc
~~~~

# Make sure Core is available in utop
~~~~
$ cat << EOM >> ~/.ocamlinit
#use "topfind";;
#thread;;
#require "core.top";;
EOM
~~~~

# Get vim to successfull load via plugin manager
~~~~
# Make sure VIM is installed. It will error the first time
$ bash ~/.vim/bin/install.sh

# Deprecated: Manually start vim, then restart vim
# $ vim
# $ :PlugInstall # in vim
~~~~

# Git stuff
in ~.gitconfig: (If you do this, don't use user/password. Use a token or SSH keys)

** NOTE - you can find the correct bin with [echo $(opam config var bin)/patdiff-git-wrapper]
~~~~
---------------------------------------------------------------------------------------
[credential]
  helper = store
[diff]
  external = /home/sagarmomin/.opam/4.10.0/bin/patdiff-git-wrapper
[user]
  email = {EMAIL}
  name = Sagar
---------------------------------------------------------------------------------------
~~~~

