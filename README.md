My personal webpage.

Link: https://luusi.github.io/

Preliminaries
Follow this guide to set up your Ruby development environment.

Install Ruby version 2.7.1:

rbenv install 2.7.1
Install bundler:

gem install bundler
Then:

bundle install
Build
To build the Jekyll website:

bundle exec jekyll build
To serve:

bundle exec jekyll serve
To publish on GitHub, from the source branch:

rake publish
