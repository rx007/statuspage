# Statuspage

[![Updates](https://pyup.io/repos/github/jayfk/statuspage/shield.svg)](https://pyup.io/repos/github/pyupio/statuspage/)
[![Build Status](https://travis-ci.org/jayfk/statuspage.svg?branch=master)](https://travis-ci.org/pyupio/statuspage)
[![codecov.io](https://codecov.io/github/jayfk/statuspage/coverage.svg?branch=master)](https://codecov.io/github/pyupio/statuspage?branch=master)

A statuspage generator that lets you host your statuspage for free on GitHub. Uses 
issues to display incidents and labels for severity. 

## Demo

![DEMO](https://github.com/pyupio/statuspage/blob/master/demo.gif)

See a real status page generated by this at [status.pyup.io](http://status.pyup.io/) or a [demo site](https://jayfk.github.io/statuspage-demo/)

## Before you start

You'll need to create a GitHub API token. Go to your 
[Personal Access tokens](https://github.com/settings/tokens) page, click on `Generate new token` and give it a description. Make
sure to check the `public_repo` scope. Copy the token somewhere safe, you won't be able to see it
again once you leave the page.


## Installation

### On Mac OS X
    curl -L https://github.com/pyupio/statuspage/raw/master/dist/statuspage_osx > /usr/local/bin/statuspage
    chmod +x /usr/local/bin/statuspage
    
### On Linux
    curl -L https://github.com/pyupio/statuspage/raw/master/dist/statuspage_linux > /usr/local/bin/statuspage
    chmod +x /usr/local/bin/statuspage

## Create a status page

To create a new status page, run

    statuspage create --token=<yourtoken>
    Name: mystatuspage
    Systems, eg (Website,API): Website, CDN, API
    
You'll be prompted for the name and the systems you want to show a status for. 

   - Name: This will be the name of the GitHub repo where your status page is hosted. It will 
   create a new GitHub repo on your account with that name, so make sure you don't use something 
   that already exists.
   - Systems: The systems you want to show a status for. This can be your website, your API, your
   CDN or whatever else you are using.


The command takes a couple of seconds to run. Once ready, it will output the URLs to the issue tracker
and your new status page

    Create new issues at https://github.com/<login>/mystatuspage/issues
    Visit your new status page at https://<login>.github.com/mystatuspage/
   
## Create an issue

To create a new issue, go to your newly created repo and click on `New Issue`.

Click on the cog icon at labels on the right. What labels you choose next will tell the generator 
about the affected system(s) and the severity. Your system's labels are all black.

Add a systems label, eg. `Website` and pick a severity eg. `major outage` and add them to the issue.

Now, fill in the title, leave a comment and click on `Submit new issue`.

Go back to your commandline and type:

    statuspage update --token=<yourtoken>
    Name: mystatuspage

This will update your status page and show a *major outage* on your *Website*.

If you change the issue (eg. when you add a new label, create a comment or close the issue), you'll
need to run `statuspage update` again.

## Use Organization Account

In order to create/update a status page for an organization, add the name of the organization to 
 the `--org` flag, e.g.:
 
     statuspage create --org=my-org --name=..
     
     
Please note: You need to have the proper permissions to create a new repository for the given
organization.

## Customizing

**Important:** All customizations have to happen in the `gh-pages` branch. If you are using the
command line, make sure to

    git checkout gh-pages
    
or, on the website, select the `gh-pages` branch before editing things.

### Template

The template is fully customizable, edit `template.html`.

### Logo

Add a `logo.png` to your repo's root and change `template.html` to point to that file.

### CSS

CSS is located at `style.css` in the root directory. Just edit it and commit the file.

### Use a subdomain

If you want to use your own domain to host your status page, you'll need to create a CNAME file
in your repository and set up a CNAME record pointing to that page with your DNS provider.

If you have e.g. the domain `mydomain.com`, your GitHub username is `myusername` and you want 
your status page to be reachable at `status.mydomain.com`


- Create a `CNAME` file in the root of your repository

        status.mydomain.com
    
- Go to your DNS provider and create a new CNAME record pointing to your

  
          Name     Type      Value 
          status   CNAME     myusername.github.io

See [Using a custom domain with GitHub Pages](https://help.github.com/articles/using-a-custom-domain-with-github-pages/) 
for more info.
