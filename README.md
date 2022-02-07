# How to build and run the developer portal.

The developer portal is built using Jekyll and runs on GitHub Pages.  More information can be found at: <a href="https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll">Testing your GitHub Pages site locally with Jekyll</a>

## Prerequisites:
  1. Have Ruby installed.  This is installed by default on MacOS. Instructions for all platforms are here: <a href="https://www.ruby-lang.org/en/documentation/installation/">Installing Ruby</a>
  2. Have Bundler installed.  Instructions are available here: <a href="https://bundler.io">Installing Bundler</a>
  3. Have Jekyll installed.  Installation instructions are available here: <a href="https://jekyllrb.com">Installing Jekyll</a>. Please note that Windows isn't officially supported by Jekyll.
  
  
## Instructions:
  1. Navigate to the base of this repo
  2. Run `bundle install`.  This will install all the required gems to run the site.
  3. Run `bundle exec jekyll serve`. This will run the local version of the site, usually at http://127.0.0.1:4000/developer/.  The output from the terminal command will show where it is running locally on your system.

