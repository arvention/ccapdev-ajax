# ccapdev-ajax
AJAX Tutorial for CCAPDEV1920T2

This repository will help you integrate AJAX to your web applications. This web application is based on the previous [repository on mongoose tutorial](https://github.com/arvention/ccapdev-mongoose). For this tutorial, we will use AJAX to verify if the ID number entered by the user in the sign-up form has not been used by any registered users yet. Using AJAX, we can check the availability of the ID number without submitting the form and refreshing the page.

## Contents:

Each folder and file in this repository is properly documented. You may read the `README.md` file of each folder to understand its content. You may also read the inline comments of each file explaining the statements line-per-line.

- [controllers](https://github.com/arvention/ccapdev-ajax/tree/master/controllers) - This folder contains files which defines callback functions for client requests.
- [models](https://github.com/arvention/ccapdev-ajax/tree/master/models) - This folder contains files for database modeling and access.
- [public](https://github.com/arvention/ccapdev-ajax/tree/master/public) - This folder contains static assets such as css, js, and image files.
- [routes](https://github.com/arvention/ccapdev-ajax/tree/master/routes) - This folder contains files which describes the response of the server for each HTTP method request to a specific path in the server.
- [views](https://github.com/arvention/ccapdev-ajax/tree/master/views) - This folder contains all hbs files to be rendered when requested from the server.
- [index.js](https://github.com/arvention/ccapdev-ajax/blob/master/index.js) - The main entry point of the web application.

## Follow the steps below to set-up and study this repository:
1. Clone the repository either by downloading the contents of the repository [here](https://github.com/arvention/ccapdev-ajax/archive/master.zip), or using the command below (Note: git should be installed in your system for this to work).
```
git clone https://github.com/arvention/ccapdev-ajax
```
2. Open Command Prompt
3. Navigate to the project folder - the folder containing the contents of the cloned or downloaded repository.
4. Run the command `npm install` to initialize and install all necessary modules used in the project.

5. We may now run our server. To do this, we run the command `node index.js`. Upon running the command, your Command Prompt should display the following statement:
```
app listening at port 9090
Connected to: mongodb://localhost:27017/ccapdev-ajax
```

6. Let's test our web application. Go to the link below to access the web application:
```
http://localhost:9090/
```

Your web browser should display the screen below:
![alt text](https://github.com/arvention/ccapdev-ajax/blob/master/index.png "Index Page")
