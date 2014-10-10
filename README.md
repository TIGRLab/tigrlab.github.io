TIGRLAB Website
---------------

This is a website for the TIGRLab, build on the Skinny Bones Jekyll Theme.

Note for n00bs like me -- you need to clone the gh-pages branch, not the master branch, if you ever want to start from scratch.

Edit \_data/???.yml to change the navigation features of the site.

Note that the 'category' field in the header of each post (under \_/posts) will determine what post type will end up in the kewl grid -- NOT the folder the post falls under (this is more for humans).

Each header / footer entry should have an identically named folder containing an index.md file.

That is all I know right now.

How I Installed on Kimsrv
-------------------------

I got some funny bundle-related errors without doing some manual monkey work, which I will detail here.
The official theme installation [instructions are available here] (https://mmistakes.github.io/skinny-bones-jekyll/getting-started/), but it isn't that simple :).

    # this is for the 'mkmf' package not available in regular ruby
    apt-get install ruby1.9.1-dev

    # this should do most of the package management for us when we want to serve locally
    gem install bundle

    # install github pages
    gem install github-pages

    # install node.js
    sudo apt-get install nodejs

    # manually install [octopress](http://sharadchhetri.com/2014/06/29/how-to-install-octopress-on-ubuntu-14-04-lts/)
    # some of this root stuff is suspicious and might be able to go...
    sudo su -
    cd /opt
    curl -sSL https://get.rvm.io | bash -s stable
    source /etc/profile.d/rvm.sh
    rvm requirements
    rvm install 2.1.2
    git clone git://github.com/imathis/octopress.git octopress
    exit; cd /opt
    chown -R [username] octopress/
    chgrp -R kimel octopress/ 
    bundle install
    rake install
 
    # install jekyll, redcarpet, 
    gem install jekyll
    gem install redcarpet

It didn't work, so hurray.

Nonetheless, you can get this frankenconfig to serve to the localhost for testing purposes in a fine-enough way using:

    jekyll serve --watch

Enjoy the madness!

