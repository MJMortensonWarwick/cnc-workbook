# GCP Serverless Python Flask Web Application

### Introduction

In the previous book we created a Linux Virtual Machine which we then installed Docker and Python on and executed some basic commands. This book looks to continue on from the previous book where we will run a Python Flask web application using Google Cloud Run. Although we will use Docker and Python we will not be installing any software or created any Virtualised environment, we will simply providing our code to Google and let them take care of the rest.

### Creating Project 

We once again start from the Google Cloud Console, and we can either continue using the project we created in the previous notebook for our Linux Virtual Machine or we can create a new one. (Be aware this project will use billable resources, and Google seems to be very restrictive with how many active billable projects you can have, so you may need to delete a project if you have reached the cap).

![GCP Dashboard](https://raw.githubusercontent.com/Jordan-Bruno/cnc-workbook/a2e787456015c8f2ac9e7d35b7f652f3ac45bf43/images/gcp-dashboard.PNG)

### Google Cloud Shell

The Google Cloud Shell is a development and operations environment which we can leverage to manage our project resources.  The standard way to interact with Google Cloud via the command line is by downloading Google Cloud Command Line Interface (gcloud CLI). To avoid any issues that may arise on individual machines we will use the Google Cloud Shell, which comes preloaded with gcloud command-line tool, Docker and Python. (It comes preloaded with lots more but this is all we will need for this exercise).

To access the Google Cloud Shell, we click on the terminal icon on the top right of the navigation bar. It is important to ensure you are on the correct project (Also on the top navigation bar, but on the left).

![Google Cloud Shell](https://raw.githubusercontent.com/Jordan-Bruno/cnc-workbook/main/images/gcp-shell.png)

Our first step is to set up a directory for us to work from:

```
mkdir webapp
cd webapp
```

We can confirm we are inside our created directory using the print working directory command:

```
pwd
```
### Creating Our Python App

Now that we are inside our created directory we will create our Python file.

```
touch main.py
```

We can confirm that our file was created successfully using the list command:

```
ls
```

Next we can build our Python application using a command line text editor called **Vim**. 

```
vim main.py
```

![Vim](https://raw.githubusercontent.com/Jordan-Bruno/cnc-workbook/main/images/gcp-vim.PNG)

To insert code into Vim we press `i` and then we can paste in our Python code.

```python
import os
from flask import Flask
from random import randint

app = Flask(__name__)


@app.route('/')
def hello_world():
    names = {1: "Mark", 2: "Liping", 3: "Jordan", 4: "Michael"}
    name = names[randint(1, 4)]
    return f"{name} says Hello 👋"


if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0', port=int(os.environ.get("PORT", 8080)))
```

After pasting in our code we save and exit out of Vim by first pressing the `ESC` key and then typing in `:wq!` (write and quit) and pressing return.

![Exiting Vim](https://raw.githubusercontent.com/Jordan-Bruno/cnc-workbook/main/images/gcp-vim-exit.PNG)

Google Cloud Shell does come with the option to open its inbuilt IDE (Google Cloud Shell Editor), here you can create and edit your files as you would on any Graphical User Interface (GUI), but, in the spirit of trying to emulate using the Google Cloud Command Line Interface, we will stick to the Cloud Shell terminal.

![Cloud Shell Editor](https://raw.githubusercontent.com/Jordan-Bruno/cnc-workbook/main/images/gcp-cloud-editor.PNG)

Next we will create a requirements file, a *requirements.txt* file stores information about all the python libraries, modules and packages we use in the development of our Python project. 

``` 
touch requirements.txt
```
Using Vim again we will edit our *requirements.txt* file.
```
vim requirements.txt
```

Add the following, remember press `i` to insert your text into the file.
```
Flask==2.1.0
gunicorn==20.1.0
```
Again press the `ESC` key and type `:wq!`  and return.

### Docker

Our next step is to build a Dockerfile:

```
touch Dockerfile
vim Dockerfile
```
Insert the following into the Dockerfile:
```
FROM python:3.10-slim

ENV PYTHONUNBUFFERED True

ENV APP_HOME /app
WORKDIR $APP_HOME
COPY . ./

RUN pip install --no-cache-dir -r requirements.txt

CMD exec gunicorn --bind :$PORT --workers 1 --threads 8 --timeout 0 main:app
```

The Dockerfile is the set of commands required to build the container required. We won't explain every single line in great detail:

```
FROM python:3.10-slim
```
We are using a fairly recent iteration of Python and a lightweight version to reduce the container size and build time. 

```
ENV APP_HOME /app
WORKDIR $APP_HOME
COPY . ./
```
Here we are setting the working directory using an environment variable and copying the code to our image container. 

```
RUN pip install --no-cache-dir -r requirements.txt
```

We are installing all of our project dependencies that our program needs to run, that are specified in our *requirements.txt* file.

```
CMD exec gunicorn --bind :$PORT --workers 1 --threads 8 --timeout 0 main:app
```
Finally we execute the gunicorn web server upon creation of our container, and set the port to the one specified in our Flask app using an environment variable.

In addition to our Dockerfile, it is good practice to include a *.dockerignore* file. A *.dockerignore* file allows you to list files that you do not wish to be included when building your docker image, although it is of little consequence to us in this case, on bigger real world projects not included a *.dockerignore* file can drastically increase the docker image size as well as its build time.

```
touch .dockerignore
vim .dockerignore
```
Enter the following into the *.dockerignore* file and save and exit:
```
Dockerfile  
README.md  
*.pyc  
*.pyo  
*.pyd  
__pycache__  
.pytest_cache
```

### Deploying Our App

With all our files built, we now only need to deploy our application using Google Cloud Run, inside your terminal (make sure you are still in your *webapp* directory) and run the following command:

```
gcloud run deploy
```

Depending on any previous tasks you have performed on your Google Project, you may need to authenticate and enable certain API's. 

The terminal will prompt you for a source code location, as we are deploying from the current folder we are in, we can simply press return.

![Source Code Location](https://raw.githubusercontent.com/Jordan-Bruno/cnc-workbook/main/images/gcp-source-code-location.png)

Next you will be prompted for a service name, press return again to use the default name.

The rest of the deployment will be either enabling and allowing various things (input `y`) and choosing a region to deploy our web application to.

After a short time, our deployment should hopefully be successful and we will be given a URL for our deployed service. Click on this URL and we should see our app that we developed. You can refresh the app and the Python code should execute every time possibly printing a different message than the previous iteration.

![Our Web App](https://raw.githubusercontent.com/Jordan-Bruno/cnc-workbook/main/images/gcp-flask-web-app.png)

### Avoiding Charges

Once we are happy with our application, we should ensure we do not incur additional charges while our application is no longer needed or in use. 

If you created a new project for this task, then the simplest way is to just delete the entire project this will ensure that all the billable resources are deleted and not in use.

Google does not charge you for your app when it is not in use, so you do not need to delete it, although you can delete your app from the **Cloud Run** page. 

However the storage of the repository may occur charges (practically nothing for our small repository), but you can delete this repository from the **Artifact Registry** page.

![Artifact Registry Page](https://raw.githubusercontent.com/Jordan-Bruno/cnc-workbook/main/images/gcp-artifact-reg.png)
