---
title: "Automate the process with Wercker"
date: 2019-09-09T22:47:16-07:00
draft: false
weight: 15
summary: Set up a Wercker pipeline to automate builds to your `gh-pages` branch.
---

The final step in this process is to automate the publishing process so each time you update your master branch on GitHub your website is automatically published to your GitHub.io page. 
### Create a Wercker account
1.  Go to [https://app.wercker.com/](https://app.wercker.com/) and create an account with the `LOG IN USING GITHUB` option.
2. Authorize Wercker to access your GitHub account.
3. Add a username and email to complete the account creation process.

### Create an application

1. From the Wercker dashboard, click the `Create your first application` button.
2. Select GitHub from the Select SCM step and click Next.
3. Select <github-username>/example-site from this list and click Next.
4. Accept the default options on the next two pages by clicking Next and then Create.
5. In the Environment tab add the following variables:

    | Key | Value | Protected|
    |-----|-----|---|
    | GIT_ACCOUNT | \<github-username> |No |
    | GIT_PROJECT | \<github-username>/example-site.git | No |
    | GIT_EMAIL | \<your-github-email> | yes|

6. In a new tab, go to https://github.com/settings/tokens and click **Generate new token**.
7. Add "Wercker" to the note field and check the `repo` box under scopes. Then click **Generate token**.
8. Copy the generated token and go back to the Environment tab on the Wercker site.
9. Add a GIT_KEY key and paste your token from GitHub into the Value field. **Select the Protected check-box** and then click Add.

### Configuring the GitHub repo

1. In a text editor, prepare the following files and save them to the `example-site` directory:

    prep.sh:
    ````
    #!/bin/bash
    git worktree add -B gh-pages public origin/gh-pages
   ````
   deploy.sh:
   ````
    #!/bin/bash
    git config --global user.email "$GIT_EMAIL"
    git config --global user.name "$GIT_ACCOUNT"
    cd public
    git add --all
    git commit -m "Publishing to gh-pages"
    cd ..
    git push https://$GIT_ACCOUNT:$GIT_KEY@github.com/$GIT_PROJECT gh-pages
    ````
    wrecker.yml
    ````
    box: debian
    build:
      steps:
    
      - install-packages:
        packages: git ssh-client
    
      - script:
            name: prep
            code: bash prep.sh
    
      - arjen/hugo-build:
            version: "0.55.6"
            theme: restaurant-hugo
            config: config.toml
    
      - script:
            name: deploy
            code: bash deploy.sh
    ````
    The `prep.sh` file is used to set up the public folder in the publishing process. The `deploy.sh` pushes the built site to the `gh-pages` branch. The `wercker.yml` ties everything together and tells the Wercker workflow how to walk through the steps.

    **Note: the `deploy.sh` file should use linux line endings. More details can be found [here]([https://help.github.com/en/articles/configuring-git-to-handle-line-endings](https://help.github.com/en/articles/configuring-git-to-handle-line-endings)).**


2. Add and commit these new files to your local repo:
    ````
    git add .
    git commit -m "Adding configuration files."
    ````
3. Now push your changes up to GitHub and check https://app.wercker.com/ to see if your site builds:
    ````
    git push origin master
    ````
4. Once successfully built, the website should update a few minutes after each commit to the source content is made.