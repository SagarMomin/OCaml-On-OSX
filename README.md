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

# Delete existing OPAM and Vim configs. Open up iTerm and let's get started.
~~~~
$ brew update
$ brew upgrade
$ brew uninstall opam
$ brew cleanup
$ rm -rf ~/.opam
$ rm -rf ~/.vim 
# Done with clean up. I'd recommend opening a new terminal
~~~~

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
$ opam init # say yes to the part where you add to your bash_profile
$ opam switch # This shows compiler versions, if default is 4.10.0 or higher, skip to opam install core utop ...
# Uncomment the next two commands if you don't see >4.11.1 as the default version
$ # opam switch create 4.11.1
$ # opam switch 4.11.1
$ opam install core utop async yojson ppx_deriving_yojson core_extended core_bench menhir depext
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

# Generally useful Linux stuff and other tools
~~~~
$ brew install coreutils fzf gzip fswatch jq nmap ripgrep p7zip tcpdump wget tmux xz
~~~~

# If you decide to install the VIM tooling, you need to use bash on OSX.
~~~~
$ brew install bash
$ sudo bash -c 'if [[ $(grep -F /usr/local/bin/bash /etc/shells >& /dev/null; echo $?) -ne 0 ]]; then echo /usr/local/b
in/bash >> /etc/shells; fi'
$ chsh -s /usr/local/bin/bash
# Done with switching to bash. I'd recommend opening a new terminal
~~~~

# Install VIM config
~~~~
# Copy over my vim-config 
$ wget 'https://drive.google.com/u/0/uc?id=1652DEDtmeaWcGTbCac0JjsD6adV5gCHN&export=download' -O vim-smomin.zip 
$ unzip vim-smomin.zip -d /tmp/vim-smomin && mv /tmp/vim-smomin/vim-config-master ~/.vim && rm -rf /tmp/vim-smomin
$ # Or if you're Sagar, glone the repo to keep it in sync
$ git clone https://github.com/SagarMomin/vim-config.git ~/.vim
~~~~

# Install opinionated bashrc and tmux configs in order to make sure everything works
~~~~
# If you don't want to overwrite your bashrc, you'll have to manually look through: ~/.vim/system-configs/setup/
$ cd ~/.vim/system-configs/ && bash smash-all-my-configs.sh && cd -
# Update inputrc to point to the one in system config
$ ln -s ~/.vim/system-configs/rcfiles/sane-defaults/inputrc ~/.inputrc
~~~~

# Make bashrc and oapm loads on terminal start
~~~~
$ cat << EOM > ~/.bash_profile
source ~/.bashrc

# opam configuration
test -r ~/.opam/opam-init/init.sh && . ~/.opam/opam-init/init.sh > /dev/null 2> /dev/null || true
EOM
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

# Testing! You did all this work, let's see if it works!
~~~~
# Let's start fresh, and open a new iTerm, terminal.
$ cdf # This takes you to the workspaces directory, for ocaml coding
$ cd app
$ git clone https://github.com/SagarMomin/hacker-rank-ocaml.git
$ cd hacker-rank-ocaml
$ tmux new -s testing-ocaml
$ cw # alias for compile-workspaces
# At this point dune should be building in the bottom right
$ vim arrays/bribes.ml
# You should now be able to navigate around. Try jumping to line 37
$ :37
# and putting your cursor over "map" in List.map, and see if merlin gives you type information [,,t]
$ ,,t
# You should see ['a list -> f:('a -> 'b) -> 'b list] at the bottom of the screen
~~~~

# Write OCaml with Vim
Some useful commands
* ```[,,t] -> Merlin type
* ```[,,y] -> Yank(copy) output of [,,t]/Merlin type
* ```[,,d] -> Show documentation for a function
* ```[,,s] -> Jump to source
* ```[,,i] -> Jump to interface
* ```[,a] -> Toggle between ml/mli/intf
* ```[,,j] -> Show all instances of a function used, in a file
* ```[,,<SPC>] -> Fzf all available keybindings
* ```[,<SPC>] -> Fzf all available Vim commands/functions
* ```[,c] -> Copy line, or visual lines into your OSX clipboard
* ```[,o] -> Ocamlformat file (should happen automatically on save)
* ```[,w] -> If you have two buffers open, diff them
* ```[,,w] -> disable buffer diff
* ```[,,l] -> Line diff. In visual mode: select lines to diff, [,l], select lines to diff against, [,l]. [,,l] when done inspecting diff to reset
  
# Git stuff
in ~.gitconfig: (If you do this, don't use user/password. Use a token or SSH keys)

** NOTE - you can find the correct bin with [echo $(opam config var bin)/patdiff-git-wrapper]
~~~~
---------------------------------------------------------------------------------------
[credential]
  helper = store
[diff]
  external = ~/.opam/default/bin/patdiff-git-wrapper
[user]
  email = {EMAIL}
  name = {NAME}
---------------------------------------------------------------------------------------
~~~~

