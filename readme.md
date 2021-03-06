# express-mailer

Send Emails from your application and response object.

[![Build Status](https://secure.travis-ci.org/RGBboy/express-mailer.png)](http://travis-ci.org/RGBboy/express-mailer)

## Note

If you have updated express-mailer from Version 0.1.2 or earlier there 
have been major API changes. The `app.sendEmail` method no longer gets 
attached to the application. Instead a mailer object is attached. The 
`app.sendEmail` functionality can now be accessed via `app.mailer.send`.

## Installation

Works with Express 3.x.x

    $ npm install express-mailer

## Usage

Express Mailer extends your express application

```javascript
// project/app.js

var app = require('express')(),
    mailer = require('express-mailer');

mailer.extend(app, {
  from: 'no-reply@example.com',
  host: 'smtp.gmail.com', // hostname
  secureConnection: true, // use SSL
  port: 465, // port for secure SMTP
  transportMethod: 'SMTP', // default is SMTP. Accepts anything that nodemailer accepts
  auth: {
    user: 'gmail.user@gmail.com',
    pass: 'userpass'
  }
});

```

## Views

Mailer views use the same render process as Express. You can use any view engine that Express supports.
Setting up views for mailer is exactly the same as setting up views for Express. For example, to set the
view directory to `project/views` and view engine to `jade` you would write:

```javascript
// project/app.js

app.set('views', __dirname + '/views');
app.set('view engine', 'jade');
```

Then we can write our templates in Jade:

```javascript
// project/views/email.jade

!!! transitional
html
  head
    meta(http-equiv = 'Content-Type', content = 'text/html; charset=UTF-8')
    title= subject
    body
      h1.h1 Lorem ipsum
      p
        strong Lorem ipsum dolor: 
        | Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
        br
        |Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
      h2.h2 Lorem ipsum
      p Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
      p Lorem ipsum:
      ol
        li Lorem ipsum dolor
        li Lorem ipsum dolor
        li Lorem ipsum dolor
```

## Sending an email

You can send an email by calling `app.mailer.send(template, options, callback)`.
To send an email using the template above you could write:

```javascript
app.get('/', function (req, res, next) {
  app.mailer.send('email', {
    to: 'example@example.com', // REQUIRED. This can be a comma delimited string just like a normal email to field. 
    subject: 'Test Email', // REQUIRED.
    otherProperty: 'Other Property' // All additional properties are also passed to the template as local variables.
  }, function (err) {
    if (err) {
      // handle error
      console.log(err);
      res.send('There was an error sending the email');
      return;
    }
    res.send('Email Sent');
  });
});
```

You can also send an email by calling `res.mailer.send(template, options, callback)`.
To send an email using the template above you could write:

```javascript
app.get('/', function (req, res, next) {
  res.mailer.send('email', {
    to: 'example@example.com', // REQUIRED. This can be a comma delimited string just like a normal email to field. 
    subject: 'Test Email', // REQUIRED.
    otherProperty: 'Other Property' // All additional properties are also passed to the template as local variables.
  }, function (err) {
    if (err) {
      // handle error
      console.log(err);
      res.send('There was an error sending the email');
      return;
    }
    res.send('Email Sent');
  });
});
```

## Sending an email with nodemailer options

You can provider nodemailer settings as well. This allows you to send attachments for instance.

```javascript
app.get('/', function (req, res, next) {
  app.mailer.send('email', {
    to: 'example@example.com', // REQUIRED. This can be a comma delimited string just like a normal email to field. 
    subject: 'Test Email', // REQUIRED.
    otherProperty: 'Other Property' // All additional properties are also passed to the template as local variables.
  },
  {
    attachments : [{fileName: 'File.extension', filePath: /path/to/file}]
  }, function (err) {
    if (err) {
      // handle error
      console.log(err);
      res.send('There was an error sending the email');
      return;
    }
    res.send('Email Sent');
  });
});
```

## Updating the configuration

You can update your original configuration by calling `app.mailer.update(updatedOptions, callback)`.
This can be processor intensive so changes to your configuration are best kept to a minimum.

## Rendering an email without sending

You can render an email without sending it by calling `app.mailer.render(template, locals, callback)`.
This can be used to check what an email will look like without sending it:

```javascript
app.get('/', function (req, res, next) {
  res.mailer.render('email', {
    to: 'example@example.com',
    subject: 'Test Email',
    otherProperty: 'Other Property'
  }, function (err, message) {
    if (err) {
      // handle error
      console.log(err);
      res.send('There was an error rendering the email');
      return;
    }
    res.header('Content-Type', 'text/plain');
    res.send(message);
  });
});
```

## Notes

Mailchimp has a bunch of templates that may be a good starting point.
Check them out at https://github.com/mailchimp/Email-Blueprints

## To Do

* Add ability to curry the from address.
* Add checking of options when .mailer.send is called.

## License 

(The MIT License)

Copyright (c) 2013 RGBboy &lt;me@rgbboy.com&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
