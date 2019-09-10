---
title: "Create a Hugo Site"
date: 2019-09-09T22:22:41-07:00
draft: false
weight: 5
summary: The first step is to set up a Hugo Site locally.
---
To prepare a Hugo site locally:

1.  Open your terminal (you can use git-bash in windows), create, and navigate into your example site:
    ```` 
    hugo new site example-site 
    cd example-site
    ````
2.  This example will use the [Restaurant Hugo theme](https://themes.gohugo.io/restaurant-hugo/). Use git to clone this theme into your `themes` folder:
    ````
    cd themes
    git clone https://github.com/themefisher/restaurant-hugo.git
    ````
    Remove this theme's git configuration to avoid issues in later steps:
    ````
    rm -rf restaurant-hugo/.git
    ````
3. Move the sample content for this theme into the project folder:
    ````
    cd ..
    cp -a themes/restaurant-hugo/exampleSite/. .
    ```` 
4. Test the site with the `hugo server` command and visit [localhost:1313](localhost:1313):
    <img src="https://i.imgur.com/JRJLuA8.png"  alt="Picture of localhost:1313" width="600" style="border-width:1px;border-color:#3087DF;border-style:solid;display: block;margin: 10px auto 0 auto;">
    <p style="text-align: center;margin-top: 3px;"><em>Success!</em></p>
5. Press `ctrl+c` in the terminal window to close down the Hugo server when done exploring the template.

Now you are ready to set up a git repo and push your site to GitHub: [Hosting the site on GitHub](../hosting-on-github/).