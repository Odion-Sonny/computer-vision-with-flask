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


During later exercises, we'll use a couple of other libraries, including `requests` (to call Computer vision service) and `python-dotenv` (to manage our keys). While we don't need them yet, we're going to make our lives a little easier by installing them now.

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
```
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Extract Text from Image</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
</head>
<body>

<div class="container">
  <h1 class="jumbotron bg-primary">Extract Text from Image</h1>
  <br><br>
  <form class="form-horizontal" action="/submit" method="post" enctype="multipart/form-data">

    <div class="form-group">
      <label class="control-label col-sm-2" for="pwd">Upload Your Image :</label>
      <div class="col-sm-10">          
        <input type="text" class="form-control" placeholder="Image URL"  name="image_url" id="pwd">
      </div>
    </div>

    <div class="form-group">        
      <div class="col-sm-offset-2 col-sm-10">
        <button type="submit" class="btn btn-success">Submit</button>
      </div>
    </div>
  </form>

	{% if prediction %}
    <h2> Predicted Text: </h2> 
    <h3 style="background-color: LightGray; padding: 70px 35px;">  {{prediction}} </h3>
    <h2>Image:</h2>
    <img src="{{img_path}}" height="400px" width="400px">
	{% endif %}

</div>
</body>
</html>
```
### Test the application

With our initial site created, it's time to test it!

- Open your terminal. 

- Run the following command to set the Flask runtime to development, which means that the server will automatically reload with every change:

```
# Windows
set FLASK_ENV=development

# Linux/macOS
export FLASK_ENV=development
```

- Run the application!
```
flask run
```

Open the application in a browser by navigating to http://localhost:5000

You should see the form displayed! Congratulations!

## Create the Computer vision service

Once the project is up and running, we will create the necessary services on Azure. We'll obtain the necessary keys to call the service, and properly store them in a .env file.

1. Get Computer vision service key

2. Create .env file to store the key

### Get Computer vision service key 

- Browse to the [Azure portal](https://portal.azure.com/#home)

- Select `Create a resource`

![image](https://user-images.githubusercontent.com/64667212/180775259-95f3efd7-447d-4a5e-b1d6-a6a11ee209ef.png)

- In the `Search` box, enter `Computer vision`

- Select `Computer vision`

- Select `Create`


- Complete the Create Computer vision form with the following values:

```
Subscription: Your subscription
Resource group:
Select Create new
Name: flask-ai
Resource group region: Select a region near you
Resource region: Select the same region as above
Name: A unique value, such as ai-yourname
Pricing tier: Free F0 
```

- Select `Review + create`

- Select `Create`

- After a few moments the resource will be created

- Select `Go to resource`

- Select `Keys and Endpoint` on the left side under `RESOURCE MANAGEMENT`

- Next to `KEY 1`, select `Copy to clipboard`

![image](https://user-images.githubusercontent.com/64667212/180775981-94e57a72-84cc-46b0-9721-6df1eccc0fb4.png)

Note: There's no difference between Key 1 and Key 2. By providing two keys you have the opportunity to migrate to new keys, by regenerating one while using the other.

### Create .env file to store the key

- Return to Visual Studio Code and create a new file in the root of the application by selecting New file and naming it `.env`

- Paste the following text into .env

```
KEY=your_key
ENDPOINT=your_endpoint
```
- Replace the placeholders

your_key with the key you copied above
your_endpoint with the endpoint from Azure
your_location with the location from Azure

- our .env file should look like the following (with your values):

```
KEY=00d09299d68548d646c097488f7d9be9
ENDPOINT=https://api.cognitiveservices.azure.com/
```


### Add code to call the service

app.py contains the logic for our application. We'll add a couple of required imports for the libraries we'll use, followed by the new route to respond to the user.

- At the very top of app.py, add the following lines of code:
```
from azure.cognitiveservices.vision.computervision import ComputerVisionClient
from azure.cognitiveservices.vision.computervision.models import OperationStatusCodes
from msrest.authentication import CognitiveServicesCredentials
import time
import os
from dotenv import load_dotenv

load_dotenv()
```
The top line will import libraries that we'll use later, when making the call to the Computer vision service. We also import `load_dotenv` from `dotenv` and execute the function, which will load the values from .env.

- At the bottom of app.py, add the following lines of code to create the route and logic for extracting text:
```
# Load the values from .env
subscription_key = os.getenv('subscription_key')
endpoint = os.getenv('endpoint')

# create a computer vision client instance
computervision_client = ComputerVisionClient(endpoint, CognitiveServicesCredentials(subscription_key))

def extractTextFromImage(image_url):

	response = computervision_client.read(url=image_url, language='en', raw=True)

	# Get the operation location (URL with an ID at the end) from the response
	read_operation_location = response.headers['Operation-Location']

	# Grab the ID from the URL
	operation_id = read_operation_location.split('/')[-1]

	read_result = computervision_client.get_read_result(operation_id)
	
	# Call the "GET" API and wait for it to retrieve the results
	while True:
			read_result = computervision_client.get_read_result(operation_id)
			if read_result.status not in ['notStarted', 'running']:
				break
			time.sleep(1)

	# create a variable to store result		
	result = ''

	# Add the detected text to result, line by line
	if read_result.status == OperationStatusCodes.succeeded:
			for text_result in read_result.analyze_result.read_results:
				for line in text_result.lines:
					result = result + " " + line.text
	return result
```


### Add a route to `app.py`
Create a route in your Flask app that calls the extractTextFromImage function whenever users submit an image URL. Then render_template to show the returned result.
```
@app.route("/submit", methods = ['GET', 'POST'])
def get_output():
	if request.method == 'POST':
		image_url = request.form.get('image_url')

	result = extractTextFromImage(image_url)

	return render_template("index.html", prediction = result, img_path = image_url)
```

Now the web app is ready. The `app.py` file will look like this-
```
from flask import Flask, render_template, request
from azure.cognitiveservices.vision.computervision import ComputerVisionClient
from azure.cognitiveservices.vision.computervision.models import OperationStatusCodes
from msrest.authentication import CognitiveServicesCredentials
import time
import os
from dotenv import load_dotenv

load_dotenv()

app = Flask(__name__)

# Load the values from .env
subscription_key = os.getenv('subscription_key')
endpoint = os.getenv('endpoint')

computervision_client = ComputerVisionClient(endpoint, CognitiveServicesCredentials(subscription_key))

def extractTextFromImage(image_url):

	response = computervision_client.read(url=image_url, language='en', raw=True)

	# Get the operation location (URL with an ID at the end) from the response
	read_operation_location = response.headers['Operation-Location']

	# Grab the ID from the URL
	operation_id = read_operation_location.split('/')[-1]

	read_result = computervision_client.get_read_result(operation_id)
	
	# Call the "GET" API and wait for it to retrieve the results
	while True:
			read_result = computervision_client.get_read_result(operation_id)
			if read_result.status not in ['notStarted', 'running']:
				break
			time.sleep(1)

	# create a variable to store result		
	result = ''

	# Add the detected text to result, line by line
	if read_result.status == OperationStatusCodes.succeeded:
			for text_result in read_result.analyze_result.read_results:
				for line in text_result.lines:
					result = result + " " + line.text
	return result


# routes
@app.route("/", methods=['GET', 'POST'])
def main():
	return render_template("index.html")



@app.route("/submit", methods = ['GET', 'POST'])
def get_output():
	if request.method == 'POST':
		image_url = request.form.get('image_url')

	result = extractTextFromImage(image_url)

	return render_template("index.html", prediction = result, img_path = image_url)
```

### Test our application

- Use Ctrl-C to stop the Flask application

- Execute the command flask run to restart the service

- Browse to http://localhost:5000 to test your application

- Enter image url into text area and select Submit.