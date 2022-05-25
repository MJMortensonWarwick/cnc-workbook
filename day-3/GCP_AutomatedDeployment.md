# Automated App Engine Deployments

### Introduction

For the CICD exercise we are going to automate app deployment to Google's  **App Engine**. Google has an official walkthrough for this which we you will follow, but for the first part due to requiring multiple software, the walkthrough has been **adapted** to be run using solely the Google Cloud Platform Console.

### Creating a New Project

Start by accessing the Google Cloud Platform Dashboard and in the top left corner click on your current project. A box should appear on your screen and in the top left corner of that box you will see the option to create a new project.

![Create New Project](https://raw.githubusercontent.com/Jordan-Bruno/cnc-workbook/main/images/gcp-new-project.png)

After naming and creating out project we need to enable the **Cloud Source Repositories API** and we can do that either by searching for the API manually in the search bar or visiting this link here:

[Enable Cloud Source Repositories API](https://console.cloud.google.com/flows/enableapi?apiid=sourcerepo.googleapis.com&redirect=https://cloud.google.com/source-repositories/docs/create-code-repository&_ga=2.265298639.1908054924.1653295303-1213745329.1647858632&_gac=1.217727844.1653295303.CjwKCAjw4ayUBhA4EiwATWyBrrL5cNkvALp0tt2LjUTQCCviQiIahLekJYX5aiVyYRtiy-MsiMlsAxoCnooQAvD_BwE)

### Creating a Repository

In the official google quick start guide it instructs us to download the **Google Cloud CLI** as well as to install the latest version of **Git** (The most popular version control software). Instead we will be using the **Google Cloud Shell** (If you recall from the last workbook, an advantage of the Google Cloud Shell is that it comes preloaded with gcloud command-line-tool and many other useful tools one being Git.

To open the Google Cloud Shell we click on the **Activate Cloud Shell** button on the right of the top navigation bar.

![Google Cloud Shell](https://raw.githubusercontent.com/Jordan-Bruno/cnc-workbook/main/images/gcp-shell.png)

After a short loading time a terminal should appear at the bottom of our screen and we should first check that this session is set to the project we just created.

![Set Project](https://raw.githubusercontent.com/Jordan-Bruno/cnc-workbook/main/images/gcp-project-set.png)

If we are currently set to a different project we can reconfigure our shell to be set to our new project using the following command:

```
gcloud config set project [PROJECT_ID]
```

After confirming we are in the correct project we can begin by creating our Google Cloud repository:

``` 
gcloud source repos create hello-world
```

We can then clone our repository and save it locally (In this case not to our machine but to our "Virtual Environment" that the Google Cloud Shell runs on).

```
gcloud source repos clone hello-world
```
### Creating a Basic Python Script

Before we create our basic python script, we should change directory to our recently created hello-world repository stored *"locally"*. 

```bash
cd hello-world
```

Similarly to the previous workbook we will continue to use the terminal to create and edit all of our files (Remember `i` to enter insert mode in the Vim text editor,  `ESC` to exit insert mode and `:wq!` to save and exit Vim.

```bash
touch main.py
```
```bash
vim main.py
```

Enter the following code into our `main.py` file:

```python
#!/usr/bin/env python

import webapp2

class MainHandler(webapp2.RequestHandler):
    def get(self):
        self.response.write('Hello, World!')

app = webapp2.WSGIApplication([
    ('/', MainHandler)
], debug=True)
```

### Creating a YAML File

YAML is a language mostly used for writing configuration files. We will create a YAML file called `app.yaml`, which will contain our configuration settings for our app to deploy.

```bash
touch app.yaml
```

Again we will use the Vim text editor to edit our YAML file and enter the following configuration information.

```bash
vim app.yaml
```

```yaml
runtime: python27
api_version: 1
threadsafe: yes

handlers:
- url: .*
  script: main.app

libraries:
- name: webapp2
  version: "2.5.2"
```

### Pushing to Cloud Source Repository

Everything we have done so far has been in our local repository so the one stored on Google Cloud Source Repository (The original one we created not the "clone") should be empty. We are now going to push our files to our Google Cloud Source Repository. 

Ensure we are still in our local `hello-world` Git directory:

```bash
pwd
```
First we must add the files to the staging area:

```git 
git add .
```

We will now commit the files to the repository, is is mandatory for a commit message to be linked with the commit and its good practice to have a brief but meaningful commit message.

```git
git commit -m "Add Hello World app to Cloud Source Repositories"
```

If this is our first time committing on the Google Cloud Shell we will be required to assign a commit author and email.

```git
git config --global user.name "Name"
```

```git
git config --global user.email "email@example.com"
```

You will need to rerun the previous commit command if you needed to assign an author. 

We have now committed to our repository and all that remains is to push the contents of our local Git repository to our Cloud Source Repository, essentially syncing the two repositories.

```git
git push origin master
```

If the push was successful, the terminal should output something similar to the following:

```
Counting objects: 21, done.
Delta compression using up to 6 threads.
Compressing objects: 100% (20/20), done.
Writing objects: 100% (21/21), 9.76 KiB | 0 bytes/s, done.
Total 21 (delta 5), reused 0 (delta 0)
remote: Storing objects: 100% (21/21), done.
remote: Processing commits: 100% (6/6), done.
To https://source.developers.google.com/p/example-project-1244/r/repo-name
 * [new branch]      master -> master
```

Finally we can view our Cloud Source Repository to confirm that the push was indeed successful, we can do this by manually searching for **Cloud Source Repositories** in the search bar on the dashboard or clicking the following link.

[Cloud Source Repositories](https://source.cloud.google.com/repos)

We should see our `hello-world` repository and if we click on it we should see our created Python and YAML file inside.

![Cloud Source Repositories](https://raw.githubusercontent.com/Jordan-Bruno/cnc-workbook/main/images/gcp-cloud-source-repo.png)

### Next Steps

Below are two official Google guides that build upon this workbook, all of the steps should be performed in the Google Cloud Shell and using the various pages on the GCP Console on the website. All of the following tasks should be completed in the same project. 

[Deploy to App Engine Guide](https://cloud.google.com/source-repositories/docs/deploy-app-engine)
[Automating App Engine Deployments](https://cloud.google.com/source-repositories/docs/automate-app-engine-deployments-cloud-build)

Remember to delete your project once you have completed all the tasks to ensure you do not deplete any of your credits unnecessarily. 

