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
mkdir serverless_webapp
cd serverless_webapp
```

We can confirm we are inside our created directory using the print working directory command:

```
pwd
```
### Creating Our Python App

Now that we are inside our created directory we will create our Python file.

```
touch app.py
```

We can confirm that our file was created successfully using the list command:

```
ls
```

Next we can build our Python application using a command line text editor called **Vim**. 

```
vim app.py
```

![Vim](test)

To insert code into Vim we press `i` and then we can paste in our Python code.

```python
TODO
```

After pasting in our code we save and exit out of Vim by first pressing the *Esc* key and then typing in `:wq!` (write and quit) and pressing return.

![Exiting Vim](test)

Google Cloud Shell does come with the option to open its inbuilt IDE (Google Cloud Shell Editor), here you can create and edit your files as you would on any Graphical User Interface (GUI), but, in the spirit of trying to emulate using the Google Cloud Command Line Interface, we will stick to the Cloud Shell terminal.

![Cloud Shell Editor](https://raw.githubusercontent.com/Jordan-Bruno/cnc-workbook/main/images/gcp-cloud-editor.PNG)

Next we will create a requirements file, a *requirements.txt* file stores information about all the python libraries, modules and packages we use in the development of our Python project. 

``` 
touch requirements.txt
```
TODO

```
vim requirements.txt
```

Add the following, remember press i....
```
Flask==2.1.0
gunicorn==20.1.0
```

save and exit...

### Docker

Create a dockerfile:

```
 touch Dockerfile
```

TODO edit dockerfile

```
vim Dockerfile
```

```
FROM python:3.10-slim  

ENV PYTHONUNBUFFERED True 
 
ENV APP_HOME /app  
WORKDIR $APP_HOME  
COPY . ./  
  
RUN pip install --no-cache-dir -r requirements.txt  
  
CMD exec gunicorn --bind :$PORT --workers 1 --threads 8 --timeout 0 main:app
```
SUMMARY OF DOCKER FILE

It is good practice to include a *.dockerignore* file, a *.dockerignore* file allows you to list files that you do not wish to be included when building your docker image, although it is of little consequence to us in this case, on bigger real world projects not included a *.dockerignore* file can drastically increase the docker image size as well as its build time.

```
touch .dockerignore
```

```
vim .dockerignore
```

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

```
gcloud run deploy
```


### TODO10: Clean Up
