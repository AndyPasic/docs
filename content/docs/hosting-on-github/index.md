---
title: "Hosting the site on GitHub"
date: 2019-09-09T22:36:20-07:00
draft: false
weight: 10
summary: Once your Hugo site is running locally, publish it on GitHub\.com.
---

Next, set up version control and publishing from a `gh-pages` branch.

### Setting up Version Control
1.  In the terminal, from the `example-site` folder, create a git repo, configure the `.gitignore` file, commit the changes locally, and push everything to GitHub:
    ````
    git init
    echo  "/public" >> .gitignore
    git add .
    git commit -m "initial commit"
    ````
2. On github.com, click the  'Start a project' button.
3. Use `example-site` for the Repository name and click the 'Create' button.
4. In your terminal, add the newly created GitHub repo as your remote origin. You can use ssh or HTTPS to authenticate with GitHub:  
    **Important: Substitute your own GitHub username in place of** `<github-username>`:
     * Using ssh authentication:
     ````
   git remote add origin git@github.com:<github-username>/example-site.git
     ````

     * Using HTTPS authentication:
        ````
       git remote add origin https://github.com/<github-username>/example-site.git
         ````

    **Note: You may need to consult [GitHub's authentication documentation](https://help.github.com/en/articles/set-up-git#next-steps-authenticating-with-github-from-git) if it has not already been configured.**

5. Push your local branch up to GitHub.com. Setting the `-u` flag will designate it as your upstream repo moving forward:
    ````
    git push -u origin master
    ````

### Setting up the `gh-pages` branch

1. Open the `config.toml`file in a text editor and update first line to: 
    ````
    baseURL = "https://<github-username>.github.io/example-site"
   ````
2. Commit the change and push it up to GitHub:
     ````
         git commit -am "Updating the baseURL."
         git push origin master
     ````
3. Create an empty gh-pages branch and push it up to GitHub:
    ````
        git checkout --orphan gh-pages
        git rm -rf . 
        git commit --allow-empty -m "Init empty branch"
        git push origin gh-pages
        git checkout master
   ````
4. Create a worktree for the `gh-pages` branch in a folder called `public`:
    ````
     git worktree add -B gh-pages public origin/gh-pages
    ````

### Publishing your website

1. Build your Hugo site locally from the root project folder:
    ````
    hugo
    ````
     <img src="https://i.imgur.com/SewthUF.png"  alt="hugo's build output" width="200">

2. Change directory into your public folder, then add and commit the generated html to the `gh-pages` branch:
    ````
    cd public
    git add .
    git commit -m "Publishing to GitHub pages."
     ````

3. Push the `gh-pages` branch up to GitHub and wait for a few minutes for your website to build.
    ````
    git push origin gh-pages
    ````
    Your site should now be live at `https://<github-username>.github.io/example-site`.

*Note: You can repeat these last three steps to manually publish the locally generated website to your gh-pages site at anytime.*

Now you are ready to automate the publishing process using Wercker: [Automating the publishing process with Wercker](../automate-with-wercker/)