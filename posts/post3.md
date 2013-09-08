
# Python Web Applications With Flask - Part III

[Last week](http://www.realpython.com/blog/python/python-web-applications-with-flask-part-ii-app-creation/) we made a functioning web tracker and user framework. We combined some flask modules and a database and worked through some simple (yet important) design principles to make our idea. This week we'll work on implementing a testing framwork, talk a bit about why testing is important and then write some tests for our application. After we'll talk a bit about debugging errors in your application and logging. Finally we'll wrap up with altering your database schema with migrations. 

From the last post your directory should look like this:

    last time
    ├── app
    │   ├── __init__.py
    │   ├── mixins.py
    │   ├── constants.py
    │   ├── users
    |   |   ├── __init__.py
    │   │   ├── constants.py
    │   │   ├── decorators.py
    │   │   ├── forms.py
    │   │   ├── models.py
    │   │   └── views.py
    │   ├── tracking
    |   |   ├── __init__.py
    │   │   ├── constants.py
    │   │   ├── decorators.py
    │   │   ├── forms.py
    │   │   ├── models.py
    │   │   └── views.py
    │   ├── static
    │   └── templates
    |   |   ├── forms
    |   |   |   └── macros.html
    |   |   ├── tracking
    |   |   |   └── index.html
    |   |   ├── users
    |   |   |   ├── login.html
    |   |   |   └── register.html
    |   |   ├── 404.html
    |   |   ├── base.html
    |   |   └── index.html
    ├── app.db
    ├── config.py
    ├── docs
    │   ├── config.html
    │   ├── pycco.css
    │   ├── run.html
    │   └── shell.html
    ├── requirements.txt
    ├── run.py
    ├── Procfile
    └── shell.py
    
If you don't want to start from scratch you can find the code [here](https://github.com/mjhea0/flask-tracking). 

## Why Test?

Before we actually write any of our tests lets talk about why testing is important. Despite what we talked about in the first post ("Simple is better than complex") your application will eventually grow in size and complexity. Web applications, in general, have many moving parts. And until you're really good at designing your applications to be beautiful and simple they'll be ugly and complicated. 

There will come a time when your code doesn't work the way you think it should. Either you just implemented something, made a change or edited someone else's code. In fact maybe you didn't make any changes and something just stopped working. How are you going to know whats actually broken and why? Running through each file line by line isn't a great solution. Running the code over and over again trying to catch the bug in the act isn't a great solution either. What if you're working on a massive coding team and touching a codebase with a few million lines of code? How do you sort out whats broken and whats not? Tests are a great way to help you figure this out. 

Tests will

- make it obvious when code is working
- when code isn't working
- what code isn't working
- what cases break the code

Everytime you go to add a feature to your application, fix a bug or change some code you should make sure your code is adequately covered by tests and that the tests all pass after you're done. 

Do:

- Add tests to cover the basic functionality of your code
- Add tests to cover as many corner cases of your code that you can think of
- Add tests to cover the corner cases you didn't think of after you go back and fix them
- Remind your coding peers to adequately test their code
- Bug people about code that doesn't pass tests

Do Not:

- Commit code without tests
- Commit code that doesn't pass or breaks tests
- Change your tests so your code passes without fixing the problem

"[Test Driven Development](http://en.wikipedia.org/wiki/Test-driven_development)" is a coding workflow/development process in which you first write tests to cover your code's functionality and then write that code. This helps define what your code should do and when you're done writing it. It also helps guarantee good test coverage. There are a couple of tools (like [coverage](https://pypi.python.org/pypi/coverage)) that can help tell you how much of your code is covered by tests. 

## Adding Tests

Now that we've worked out why testing is so important lets start writing some tests for our application. 

Each chunk of functionality needs to have testing. To do this in a neat and concise way each module will get a ```tests.py``` file in it. This way we know where the tests for each module are and they're contained in the module if we ever need to break it out of our application. 

Let's start with the ```Users``` module. 

### Users

From last time we know that "Users sign up/log in and out". So we need to write tests that cover users signing up, users loggin in and then users logging out. 

We'll use ```Flask-Testing``` extension because it has a bunch of useful testing features that we'd be setting up anyways. 

Go ahead and add ```Flask-Testing==0.4``` to the bottom of ```requirements.txt``` then run ```pip install -r requirements.txt```. This should install Flask-Testing. 


