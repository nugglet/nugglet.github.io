# Code Nuggets by nugglet

Personal blog and thought emporium.


## To run locally (not on GitHub Pages, to serve on your own computer)

1. Clone the repository and made updates as detailed above
1. Make sure you have ruby-dev, bundler, and nodejs installed: `sudo apt install ruby-dev ruby-bundler nodejs`
1. Run `bundle clean` to clean up the directory (no need to run `--force`)
1. Run `bundle install` to install ruby dependencies. If you get errors, delete Gemfile.lock and try again.
1. Run `bundle exec jekyll liveserve` to generate the HTML and serve it from `localhost:4000` the local server will automatically rebuild and refresh the pages on change.

# Debugging Help

- Ruby not recognized as internet or external command:
    - Add ruby to path
    - in an elevated terminal run `gem update --system`
    - run `gem install bundler`
    - `bundle exec jekyll liveserve`
- Jekyll Serve doesnt work:
    - Make sure to do `bundle add webrick`
    - run `gem cleanup`
    - try `jekyll serve -w`
    -  https://stackoverflow.com/questions/37055554/jekyll-serve-not-working
- if you are using this repo and now get a notification about a security vulnerability, delete the Gemfile.lock file. 


Design References:
- https://github.com/mmistakes/minimal-mistakes
- https://mmistakes.github.io/minimal-mistakes/
- https://academicpages.github.io/