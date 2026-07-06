TIGRLAB Website
---------------

This is TIGRLab's website, built on the Skinny Bones Jekyll Theme.

Note -- you need to clone the gh-pages branch, not the master branch, if you ever want to start from scratch.

Edit \_data/???.yml to change the navigation features of the site.

Note that the 'category' field in the header of each post (under \_/posts) will determine what post type will end up in the kewl grid -- NOT the folder the post falls under (this is more for humans).

Each header / footer entry should have an identically named folder containing an index.md file.


Running the site locally for testing
------------------------------------
The best way to run the site locally if you want to test some changes is with Docker.

```bash
    # First clone the repo
    git clone https://github.com/TIGRLab/tigrlab.github.io.git

    # Switch to the site directory
    cd tigrlab.github.io

    # Make sure the 'gem' dependencies are installed
    docker run --rm -it \
        -v "$PWD":/srv/jekyll \
        -v jekyll_bundle:/usr/local/bundle \
        jekyll/jekyll:pages bundle install

    # Start the server with the 'dev' config and jekyll set to production mode
    # so resources serve correctly
    docker run --rm -it \
        -v "$PWD":/srv/jekyll \
        -v jekyll_bundle:/usr/local/bundle \
        -p 4000:4000 \
        -e JEKYLL_ENV=production \
        jekyll/jekyll:pages bundle exec jekyll serve \
            --verbose \
            --watch \
            --force_polling \
            --config _config.yml,_config_dev.yml
```

