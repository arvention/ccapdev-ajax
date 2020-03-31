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

7. Go to the sign-up page either using the menu item or the button. Enter sample user details. In this web application, we will use the ID number as a key - thus it should be unique for each user. Copy the sample user details below.

The picture below displays a properly filled form:
![alt text](https://github.com/arvention/ccapdev-ajax/blob/master/signup.png "Sign-up Page")

8. The web application should display the success screen. This screen displays a welcome message - displaying the first name, the last name, and a link to the profile of the registered user.

The picture below displays the success screen for the details that we have entered earlier:
![alt text](https://github.com/arvention/ccapdev-ajax/blob/master/success.png "Success Page")

If you have entered the same sample details, the URL displayed by your web browser should be:
```
http://localhost:9090/success?fName=Ned&lName=Stark&idNum=11312345
```

9. Go to the sign-up page again either using the menu item or the button, and try to register another user with the same ID Number. Upon entering the same ID number, the ID Number field will turn red, the submit button will be disabled, and an error message saying the "ID number already registered" will be displayed. Deleting the last number or entering another number in the field restores the HTML elements back to its previous state.

The picture below displays the state of the sign-up page when a user enters a previously registered ID number:
![alt text](https://github.com/arvention/ccapdev-ajax/blob/master/signup-ajax.png "Sign-up Page Duplicate ID Number")

The web application does this asynchronously, communicating with the server and its database in the background, without even needing to submit the form and refresh the page. This is done through the use of AJAX.

We will discuss each line of code which defines this behavior.

Review the file [`views\signup.hbs`](https://github.com/arvention/ccapdev-ajax/blob/master/views/signup.hbs), focus on the `<form>` element, and take note of its elements and their attributes. Shown below is the `<form>` as excerpted from the file:

```
<form id="signup" method="post">
    <input type="text" name="fName" id="fName" class="field" placeholder="First Name" required> <br>
    <input type="text" name="lName" id="lName" class="field" placeholder="Last Name" required> <br>
    <input type="number" name="idNum" id="idNum" class="field" placeholder="Id Number" required> <br>
    <input type="password" name="pw" id="pw" class="field" placeholder="Password"> <br>
    <input type="submit" id="submit" value="SUBMIT">
</form>
```

The client-side script [`public\js\signup.js`](https://github.com/arvention/ccapdev-ajax/blob/master/public/js/signup.js) is attached to [`views\signup.hbs`](https://github.com/arvention/ccapdev-ajax/blob/master/views/signup.hbs). This script contains lines of code which attaches a `keyup()` event to the `<input>` element where `id` is equal to `idNum`, i.e. the field which accepts the ID number of the user. Shown below is the code as excerpted from the file:

```
$('#idNum').keyup(function () {

    var idNum = $('#idNum').val();

    $.get('/getCheckID', {idNum: idNum}, function (result) {

        if(result.idNum == idNum) {
            $('#idNum').css('background-color', 'red');
            $('#error').text('ID number already registered');
            $('#submit').prop('disabled', true);
        }

        else {
            $('#idNum').css('background-color', '#E3E3E3');
            $('#error').text('');
            $('#submit').prop('disabled', false);
        }
    });
});
```

In this file, we have defined that each time a user enters a character in the `<input>` element for ID number, we have to execute the callback function. The callback function gets the current value in the field, then sends an HTTP GET request using the AJAX method `$.get()`. The first parameter is the path that we want to access in the server, the second argument is an object containing the values that we want to pass to the server, and the last parameter is a callback function to be executed when the client responds.

We send the current ID number entered by the user in the `<input>` field to the server as argument to the second parameter of the AJAX method `$.get()`. The server will reply with the same ID number if the ID number exists in the database - thus prompting the form to change its apperance, disable the submit button, and display an error message. If the ID number is not yet registered in the database, then the server will just return an empty string - restoring the form to previous state.

Upon sending the HTTP GET request with the path `/getCheckID`, the server will try to match this path to the paths defined in [`routes\routes.js`](https://github.com/arvention/ccapdev-ajax/blob/master/routes/routes.js). Shown below is a line as excerpted from the file:

```
app.get('/getCheckID', signupController.getCheckID);
```

When the server receives an HTTP GET request for the path `/getCheckID`, it executes the function `getCheckID()`. Check the file [`controllers\signUpController.js`](https://github.com/arvention/ccapdev-ajax/blob/master/controllers/signUpController.js) and focus on the function `getCheckID()`. Shown below is the function as excerpted from the file:

```
getCheckID: function (req, res) {

    var idNum = req.query.idNum;

    db.findOne(User, {idNum: idNum}, 'idNum', function (result) {
        res.send(result);
    });
}
```

As previously discussed, the values passed using the HTTP GET method is retrieved through the `req.query` object. The `getCheckID()` function uses the function `findOne()` of the file [`models\db.js`](https://github.com/arvention/ccapdev-ajax/blob/master/models/db.js) to check if the ID number has been previously registered with another user. If the ID number exists in the database, it will return it back to the caller, otherwise it will return an empty string.

10. Read the rest of the documentation in the `README.md` files in each folder and in the in-line comments in each file. Try registering various users with the same ID number and check if the web application will detect that the ID number has been used by another user :sunglasses:
