# Setup
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

### Starting from scratch
This copy of the website is entirely local to your computer. You can test out any changes you want. If you make a mistake and can't figure out how to fix it, just nuke the folder and restart from scratch. You won't need to rerun "bundle install", and none of your changes will be saved to Github.
```
rm -r ~/git/melscritters
cd ~/git
git clone https://github.com/ejalpr/melscritters.git
cd melscritters
bundle exec jekyll serve
```
And re-navigate to localhost:4000.

# Editing the website: Basics
Keep "bundle exec jekyll serve" running in a terminal window in the background. Open up a new window.
```
cd git/melscritters
```
### Adding or editing pages (like Gallery)
Example: Credits
```
nano /pages/credits.md
```
Every file starts with front matter to change basic variables. Some of these are mandatory, others are optional
```
---
layout: page ## uses this template from /\_layouts. Usually, you'll want to use 'page'.
title: Gallery ##self-explanatory. *MANDATORY
order: 2 ## If this page appears on the sidebar, what order do you want it in?
permalink: /credits ## will place the page at melscritters.com/credits *MANDATORY
hide: true ## "true" if you want to hide the page from the sidebar. Don't type this line if you would like it to appear.
cover-photo: /pictures/banner.jpg ## uses the listed picture as a cover photo. I actually don't think this does anything outside the main page but I was too lazy to delete it.
cover-photo-alt: ## Description of cover-photo, if you choose to include it.
icon: fa-image ## Uses this icon in the sidebar. Go to "https://fontawesome.com/icons?d=gallery&q=image&m=free" for a collection of icons you can choose from. Not all of them work, but most of the simple ones do. When you choose one, identify it as fa-[icon-name].
---
```
Below the dashes goes your content. You can type in Markdown for simple formatting, images, etc (https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet), or add in html, css, and javascript (see /pages/gallery.md as an example).

ctrl-x to exit nano, it'll ask you at the bottom if you want to save. n/y, then enter.

Try to add a new page:
```
nano pages/new.md
```
And add:
```
---
layout: page
title: New Page
order: 3
permalink: /example
icon: fa-android
---
This is a new page.

```
ctrl-x and save. Go to localhost:4000/example to see your new page.

## Uploading new pictures
Pictures go in the melscritters/pictures folder. Your Windows installation is located at "/mnt/c/" (replace c with your drive letter), so you can copy any files over from Windows.

For example, if you have rat.jpg in your Downloads folder and you want to add it to your website:
```
cp /mnt/c/Users/Wesleigh/Downloads/rat.jpg ~/git/melscritters/pictures/rat.jpg
```
Note that the Windows folders in the path are case-sensitive.

You can bulk copy files or folders like this, using the normal Linux commands you know.

## Changing the Home Page (including the header photo)
Each section on the homepage is a separate file in \/sections.

The cover photo is identified in Intro, so you'll change it there.

nano /\_sections/intro.md
```
---
title: Intro
order: 1 ## The order you'd like the section to appear on the home page
cover-photo: /pictures/cover2.png ## Adds a background photo to this section. For most sections, leave this off. Add it for Intro.
cover-photo-alt: banner ## If you include a cover-photo
icon: fa-comment
auto-header: none ## Without this line, the title will be added automatically. If you want to hide the title (for example, to show a banner photo instead), add this line.
---
Content content content
```
In intro.md and intro2.md, the \<style> and \<div> are what put those buttons there and show/hide them depending on the screen size. If you want to change where the buttons go, you'll find the \<button> \</button> tags at the bottom.

You can add or delete sections on the homepage by adding or deleting them in the /\_sections/ folder.

Try opening your About.md section. You'll see all the content and can edit it however you'd like.

## Changing your Gallery
```
nano pages/gallery.md
```
A few lines down, you'll see a bunch of divs. To add new photos or to change existing photos, you'll need to change 3 places. Mostly, it's just copying and pasting - just keep the formatting matching the existing formatting and make sure you're working inside the right divs.

1. Inside \<!-- INITIAL IMAGES -->

```
<img src="/pictures/rat_couple.jpg" style="width:100%" onclick="openModal();currentSlide(9)" class="hover-shadow">
```
Change 'rat_couple.jpg' to the name of the picture you uploaded. Change 'currentSlide(#)' to the matching number (see below). Make sure you're not repeating any numbers!

You'll notice that inside \<div class="row"> there are four separate \<div class="column c1/2/3/4">. Adding the new entry within one of these columns will place it in that column of photos on the page - four columns, assuming your browser window is big enough. Shrinking the window or going to mobile will automatically reshuffle it into two columns, then into 1 column.

2. Inside \<!-- POPUP IMAGES -->

```
<div class="mySlides">
  <div class="numbertext">2 / 9</div>
  <img src="/pictures/bunny_jackalope.jpg" style="width:100%">
</div>
```
Change "# / 9" to match the number in part 1. This is the number that appears in the upper left-hand corner of the picture that pops-up when you click. Change the \<img src> to your picture location. Make sure each entry is in its own separate \<div class="mySlides"> entry.

Note, these placements will determine what order pictures appear in the slideshow, which can be different from the orders pictures appear in the 4 columns. All you have to do is make sure the number for each picture matches.

In theory, you could use a low-res copy in section 1 and a high-res copy in section 2, but I just used the same file for each picture. Assuming it's big enough, it'll appear in good quality on both.

3. Picture Caption: Inside \<!-- CAPTIONS -->
```
    <div class="column">
      <img class="demo" src="/pictures/black.jpg" style="width:0%" onclick="currentSlide(9)" alt="Rat 
Couple">
    </div>
```
DON'T change black.jpg here. It's just a placeholder so the caption appears on a black background. But change 'alt="NAME"' to the caption you'd like to show underneath each pop-up, and change 'currentslide(#)' to match the number you put in the other two sections.

## Changing your Featured Critters
```
nano _sections/featured.md
```
You'll want to change this in two places.

1. Under \<!-- MAIN PHOTO -->
```
  <div class="mySlides fade">
    <div class="numbertext">1 / 4</div>
      <img src="/pictures/octopus_classof2018.jpg" style="width:100%" onclick="url()">
  </div>
```
Change the image location, like before.

2. Under \<!-- THUMBNAIL -->
```
    <div class="columnsg">
      <img class="demo cursor" src="/pictures/octopus_classof2018.jpg" style="width:100%" onclick="currentSlide(1)" alt="Octopus staring into the distance">
    </div>
```
Change the image location and, if necessary, the currentSlide(#) to match the above. Also change the alt-text for the caption.

With these, it's best to replace the existing photos rather than add new ones - the thumbnails at the bottom don't play nice if you have the wrong number of photos there. I think it is changable, to an extent, but required going into the css, so I'd recommend just leaving it at 4 photos.

Some notes here: url() is set to send people to the Gallery url on click right now; you can change this in assets/js/slideshow.js. Also, right now, it's set on a timer to cycle through photos every 10 seconds or so. You can change this time or get rid of it completely, in the same js file. It works well, but the only weirdness is, if you manually choose a different slide right before the timer is set to change, the timer will go off and change the picture away from the one you just chose. I'm sure there's a way to reset the timer to 0 on every click, but I don't know how to do it yet. If you choose a long enough timer (maybe 25,000 milliseconds), it'll probably happen rarely enough that it won't matter.

# More Complex Changes

### Templates
You shouldn't need to change any of the template folders, but here's an overview just in case you'd like to experiment:
* /\_includes contains snippets of code that are reused on multiple pages. So, for example, I wrote \_includes/custom_social.html to add in an Etsy icon. Another page, \_includes/footer.html, includes a footer on every page. You get the idea.
* /\_layouts has the base formatting for each of the various page types. Here's where you can call your /\_includes files. Every new page you make use one of these files as its beginning default.
* /\_posts is where you would store posts if you wanted to write a blog.
* /\_sass contains css code.
* /\_sections contains the main sections you see on the front page (Intro, Featured, About, Connect).
* /assets contains extra js, css, fonts, and some default images. So, for example, the javascript and css controlling the slideshows are located in /assets/js/modal.js and slidegallery.js, and /assets/css/modal.css and slidegallery.css
* /pages contains all the other pages on the website, right now Gallery, Credits, and Index (which is a blank container to hold the front page). You can have these extra pages appear in the sidebar (like Gallery) or not (like Credits).
* /pictures is where I store all the pictures
* Most of the other files are administrative, but \_config.yml contains some global variables: You could change the website's title, subtitle, social urls, avatar, etc.

# Saving Your Changes
After you've double-checked that everything's working in your local copy, use these commands to upload them to the repository on Github.

WARNING! This will upload all the changes you've saved, so make sure you haven't done anything you don't want to make permanent.

Your changes won't be uploaded until you run the commands.

First time only:
```
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```

To upload your changes
```
git add .
git commit -m "Some comment to say what you've changed"
git push https://github.com/ejalpr/melscritters master
```

If you'd like me to make changes in the future

# Further Reading
If you want to know more about the inner workings of the website:

Github Pages: https://help.github.com/articles/what-is-github-pages/

Jekyll: https://jekyllrb.com/docs/home/

The Prologue theme which I forked, which has more information about how to customize your website: https://github.com/chrisbobbe/jekyll-theme-prologue
