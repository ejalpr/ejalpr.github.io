# Instructions
## Install Windows Subsystem for Linux (WSL)
Instructions: https://docs.microsoft.com/en-us/windows/wsl/install-win10  
You can choose any distro, but I'm assuming Ubuntu 18.04 for these instructions (https://www.microsoft.com/store/productId/9N9TNGVNDL3Q)  
After you install it, launch Ubuntu from your start menu.  
From here on, run everything in the Ubuntu terminal window.

## Install Dependencies (see https://jekyllrb.com/docs/installation/ for details or troubleshooting)
Run these in the WSL Ubuntu terminal window
```
sudo apt-get install ruby ruby-dev build-essential libgmp-dev patch zlib1g-dev liblzma-dev
```

(Don't use sudo for these. Allows you to run ruby gems without elevated privileges)

```
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME=$HOME/gems' >> ~/.bashrc
echo 'export PATH=$HOME/gems/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```
Install Jekyll (the platform that the website runs on):
```
gem install jekyll bundler
```

## Setup git
git should be preinstalled on WSL, but "sudo apt-get install git" if you need to
```
cd ~
mkdir git
cd git
git clone https://github.com/ejalpr/melscritters.git
cd melscritters
```
Now, if you run "ls" in melscritters, you should see all the website files.

```
bundle install
```
