Title: Getting started tutorial
Date: 2024-09-15 00:00
Category: Tutorial

The purpose of this tutorial is to generatic a static site using Pelican, to add a new article to it, and to host it on Github Pages.

## Step 1: Create a GitHub Account and Repository

1. **Ensure you have a GitHub account.** If you don't have one, sign up at [GitHub](https://github.com/). A GitHub account is required to proceed.

2. Once logged in, create a new repository with your desired name, e.g. `my-pelican-site`.

> Note: Keep this page open, as you'll need to configure the repository in a later step.

## Step 2: Creating Your Site

[Install Pelican](https://docs.getpelican.com/en/latest/install.html)

```bash
python -m pip install "pelican[markdown]"
```

If you haven't already, set up your Pelican site:

```bash
pelican-quickstart
```

Follow the prompts to configure your Pelican site.

```bash
Where do you want to create your new web site? [.] 
What will be the title of this web site? my-pelican-site
Who will be the author of this web site? me
What will be the default language of this web site? [en] en
Do you want to specify a URL prefix? e.g., https://example.com   (Y/n) n
Do you want to enable article pagination? (Y/n) n
What is your time zone? [Europe/Rome] Europe/Dublin
Do you want to generate a tasks.py/Makefile to automate generation and publishing? (Y/n) y
Do you want to upload your website using FTP? (y/N) n
Do you want to upload your website using SSH? (y/N) n
Do you want to upload your website using Dropbox? (y/N) n
Do you want to upload your website using S3? (y/N) n
Do you want to upload your website using Rackspace Cloud Files? (y/N) n
Do you want to upload your website using GitHub Pages? (y/N) n
```

## Step 3: View your site

From within your project run the following and then access `http://127.0.0.1:8000/` in your browser,

```bash
pelican --autoreload --listen

open http://127.0.0.1:8000/
```

This will generate the static files in the output directory. Any changes made to your side should be immediately reflected within your browser.

## Step 4: Adding content to your website

Within the `content` folder - create a new piece of content e.g. `hello.md`

```markdown
Title: My first article
Date: 2024-09-18 18:00
Category: Hello

# This is my first article

Hello
```

Within your browser, the following should now be shown.

For more on writing content: view [writing content](https://docs.getpelican.com/en/latest/content.html) from Pelican.

## Step 5: Configure a Github action to perform the deployment

The next step is to prepare aGithub action that will be responsible for publishing your site.

To run this action, you will need to enable `Read and write permissions` within `Settings -> Actions -> General`` on the Repository. This is configured using the Github repository created during step 1.

Within the project folder, you will need to create a `.github` folder and a `workflow` folder within it before adding following Github action to `.github/workflows`. You can save this file `publish.yml`.

```yaml
name: Deploy Github Pages

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4.1.7
      - uses: nelsonjchen/gh-pages-pelican-action@0.2.0
        env:
          GITHUB_TOKEN: ${{secrets.GITHUB_TOKEN}}
          PELICAN_CONFIG_FILE: publishconf.py

```

## Step 6: Upload your static site to Github

Add your repository as a remote and push all your changes

```bash
git init
git add .
git remote add origin <https://github.com/username/my-pelican-site.git
git push origin head
```

## Step 7: Configure GitHub Pages

1. Go to your repository on GitHub (<https://github.com/username/my-pelican-site>).
2. Navigate to Settings > Pages.
3. Under Source, select the gh-pages branch.
4. Save the settings. GitHub will display the URL where your site is published, which will be <https://username.github.io/my-pelican-site>.

## Step 8: Access Your Site

Within the `Actions` tab on the Github repository created, you can view the jobs run to identify if it was published sucessfully or not.

Once GitHub has built and published your site, it will be accessible at:

```bash
<https://username.github.io/my-pelican-site>
```

## Step 9: Next Steps

You have now covered the basics of creating a static site, adding content, and publishing it. Why not explore the following to see how you can take your project further.

* Explore Pelican [Themes](https://docs.getpelican.com/en/latest/themes.html)
* Add a [Custom 404 Page](https://docs.getpelican.com/en/latest/tips.html)
* Explore the [internals](https://docs.getpelican.com/en/latest/internals.html)
