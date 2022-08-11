# Build an AI web app to Extract Text From Image using Flask and Azure’s Computer Vision API

## Learning objectives

- Learn how to set up a Flask development environment.
- Learn how to use Flask to build a web app.
- Learn how to use the Computer Vision service to extract text.

## Set up a development environment

To get started writing our Flask application with Python, we need to set up our development environment, which will require a couple of items to be installed. Fortunately, the tools we'll use are relatively common, so they'll serve you well even beyond this module. You might even have them installed! We'll use these tools to develop and test your application locally.

At a high level, we'll perform the following steps:

1. Install Visual Studio Code (if not already installed)
2. Install Python (if not already installed)
3. Create a directory for your code
4. Create a virtual environment
5. Install Flask and other libraries

### Install Visual Studio Code (if not already installed)

Visual Studio Code is an open-source code editor that allows you to create almost any type of application you might like. It's backed by a robust extension marketplace where you can find add-ons to help make your life as a developer easier.

### Install Python (if not already installed)

To complete this unit, you must have Python 3.6 or later installed on your computer. There's a chance you might already have Python installed, especially if you've already used it. You can confirm whether it's installed by executing one of the following commands:

```
python --version
```

### Create a directory for your project/code

Create a directory in the location of your choice. This directory will be your project directory, and will contain all of the code we'll create. You can create a directory from a command or terminal window with one of the following commands:

```
# Windows
md contoso
cd contoso

## macOS or Linux
mkdir contoso
cd contoso
```

### Create a virtual environment

A Python virtual environment isn't necessarily as complex as it sounds. Rather than creating a virtual machine or container, a virtual environment is a folder that contains all of the libraries we need to run our application, including the Python runtime itself. By using a virtual environment, we make our applications modular, allowing us to keep them separate from one another and avoid versioning issues. As a best practice you should always use virtual environments when working with Python.

To use a virtual environment, we'll create and activate it. We create it by using the venv module, which you installed as part of your Python installation instructions earlier. When we activate it, we tell our system to use the folder we created for all of its Python needs.

```
# Windows
# Create the environment
python -m venv venv
# Activate the environment
.\venv\scripts\activate

# macOS or Linux
# Create the environment
python -m venv venv
# Activate the environment
source ./venv/bin/activate
```

### Install Flask and other libraries

With our virtual environment created and activated, we can now install Flask, the library we need for our website. We'll install Flask by following a common convention, which is to create a `requirements.txt` file.

The `requirements.txt` file isn't special in and of itself; it's a text file where we list the libraries required for our application. But it's the convention typically used by developers, and makes it easier to manage applications where numerous libraries are dependencies.


During later exercises, we'll use a couple of other libraries, including `requests` (to call Translator service) and `python-dotenv` (to manage our keys). While we don't need them yet, we're going to make our lives a little easier by installing them now.

- Open the folder in Visual Studio Code.

- Create a new file.

- Name the file `requirements.txt`, and add the following text:

```
Flask
azure-cognitiveservices-vision-computervision
msrest
python-dotenv
requests
```

- Save the file by clicking Ctrl-S, or Cmd-S on a Mac

- Return to the command or terminal window and perform the installation by using pip to run the following command:

```
pip install -r requirements.txt
```

Hurray!!! The command downloads the necessary libraries and their dependencies.


## Create the app
After setting up the environment, we will create the project. We will create the template for the landing page and test that the application is running correctly.

Typically, the entry point for Flask applications is a file named `app.py`. We're going to follow this convention and create the core of our application. We'll perform the following steps:

1. Create our core application
2. Add the route for our application
3. Create the HTML template for our site
4. Test the application

### Create our core application

Returning to the instance of Visual Studio Code we were using previously, create a new file named `app.py` by clicking `New file` in the `Explorer` tab

![image](https://user-images.githubusercontent.com/64667212/180769043-ffa160c4-d806-421b-aaa8-41dc5087ceaf.png)

Add the code to create your Flask application

```
from flask import Flask, redirect, url_for, request, render_template, session

app = Flask(__name__)
```

The import statement includes references to `Flask`, which is the core of any Flask application. We'll use `render_template` in a while, when we want to return our HTML.

`app` will be our core application. We'll use it when we register our routes in the next step.

### Add the route for our application

Our application will use one route - /. This route is sometimes called either the `default` or the `index` route, because it's the one that will be used if the user doesn't provide anything beyond the name of the domain or server.

Add the following code to the end of `app.py`

```
@app.route('/', methods=['GET'])
def index():
    return render_template('index.html')
```

By using `@app.route`, we indicate the route we want to create. The path will be /, which is the default route. We indicate this will be used for GET. If a GET request comes in for /, Flask will automatically call the function declared immediately below the decorator, `index` in our case. In the body of `index`, we indicate that we'll return an HTML template named `index.html` to the user.


### Create the HTML template for our site

Jinja, the templating engine for Flask, focuses quite heavily on HTML. As a result, we can use all the existing HTML skills and tools we already have. We're going to use Bootstrap to lay out our page, to make it a little prettier. By using Bootstrap, we'll use different CSS classes on our HTML. If you're not familiar with Bootstrap, you can ignore the classes and focus on the HTML (which is really the important part).


Templates for Flask need to be created in a folder named templates, which is fitting. Let's create the folder, the necessary file, and add the HTML.

1. Create a new folder named `templates` by selecting `New Folder` in the `Explorer` tool in Visual Studio Code

2. Select the `templates` folder you created, and select `New File` to add a file to the folder

3. Name the file `index.html`

4. Add the following HTML
