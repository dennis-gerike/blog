# Discover Quality

This repository holds the code and content for my QA blog -> dennis-gerike.de.
It is powered by the static site generator [Hugo](https://gohugo.io/).

## Getting Started

* fetch a copy of this repository
    * e.g. run `git clone https://github.com/dennis-gerike/blog`
* install Hugo
    * see installation manual for every supported OS: https://gohugo.io/installation/
    * the extended version of Hugo should be installed to be able to compile the SASS files
* start Hugo
    * run `hugo server -DF`
        * argument D to shows draft articles
        * argument F to show articles from the future
    * in the console output the URL to the web server is printed
        * should be http://localhost:1313/ if the port is not already used

## Compiling a new version

* run `hugo`
* only the public folder should now have changes