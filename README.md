# Instructions
## Install Windows Subsystem for Linux (WSL)
* Instructions: https://docs.microsoft.com/en-us/windows/wsl/install-win10. Essentially, activate it in Optional Features, then install a distro from the Windows Store.
* You can choose any distro, but I'm assuming Ubuntu 18.04 for these instructions (https://www.microsoft.com/en-us/p/ubuntu-1804/9n9tngvndl3q).
* After you install it, launch Ubuntu from your start menu.
* From here on, run everything in the Ubuntu terminal window.

## Install Dependencies
See https://jekyllrb.com/docs/installation/ for details or troubleshooting.

Dependencies (note, I needed more dependencies than the Jekyll website says, since our theme uses a gem called nokogiri):
```
sudo apt-get install ruby ruby-dev build-essential libgmp-dev patch zlib1g-dev liblzma-dev
```

Don't use sudo for these next commands; they'll allow you to run Ruby Gems without elevated privileges. Run:

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
git should be preinstalled on WSL, but "sudo apt-get install git" if you need to.

Then, clone the repo onto your computer:
```
cd ~
mkdir git
cd git
git clone https://github.com/ejalpr/melscritters.git
cd melscritters
```
Now, if you run "ls" in melscritters, you should see all the website files.  

Install the necessary gems on your system. Be patient, "bundle install" takes a long time, and especially has a tendency to hang when installing nokogiri.
```
cd ~/git/melscritters
bundle install
bundle update
```

Now start your server:
```
bundle exec jekyll serve
```

And navigate to http://localhost:4000/ in your web browser. Voila! Hit ctrl-c when you're ready to shut the server down.

This copy of the website is entirely local to your computer. You can test out any changes you want. If you mess things up beyond repair, just nuke the folder and restart from scratch. You won't need to rerun "bundle install".
```
rm -r ~/git/melscritters
cd ~/git
git clone https://github.com/ejalpr/melscritters.git
cd melscritters
bundle exec jekyll serve
```
And re-navigate to localhost:4000.

## Editing the website
Keep "bundle exec jekyll serve" running in a terminal window in the background. Open up a new window.
```
cd git/melscritters
```
### Uploading new pictures
Pictures go in the melscritters/pictures folder. Your Windows installation is located at "/mnt/c/" (replace c with your drive letter), so you can copy any files over from Windows.

For example, if you have rat.jpg in your Downloads folder and you want to add it to your website:
```
cp /mnt/c/Users/Wesleigh/Downloads/rat.jpg ~/git/melscritters/pictures/black.jpg
```
Note that the Windows folders in the path are case-sensitive.

You can bulk copy files or folders like this, using the normal Linux commands you know.

You shouldn't need to change any of the template folders, but here's an overview just in case you'd like to experiment:
* /\_includes contains snippets of code that are reused on multiple pages. So, for example, I wrote \_includes/custom_social.html to add in an Etsy icon.
* /\_layouts has the base formatting for each of the various page types.
* /\_posts is where you would store posts if you wanted to write a blog.
* /\_sass contains css code.
* /\_sections contains the main sections you see on the front page (Intro, Featured, About, Connect).
* /assets contains extra js, css, fonts, and some default images. So, for example, the javascript and css controlling the slideshows are located in /assets/js/modal.js and slidegallery.js, and /assets/css/modal.css and slidegallery.css
* /pages contains all the other pages on the website, right now Gallery, Credits, and Index (which is a blank container to hold the front page). You can have these extra pages appear in the sidebar (like Gallery) or not (like Credits).
* /pictures is where I store all the pictures
* Most of the other files are administrative, but \_config.yml contains some global variables: You could change the website's title, subtitle, social urls, avatar, etc.
