# grunt-nodemailer

> Grunt wrapper for [Nodemailer](https://github.com/andris9/Nodemailer)



## Getting Started
This plugin requires Grunt `~0.4.1`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-nodemailer --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-nodemailer');
```

## The "nodemailer" task

### Overview
In your project's Gruntfile, add a section named `nodemailer` to the data object passed into `grunt.initConfig()`.

```js
grunt.initConfig({
  nodemailer: {
    options: {
      // Task-specific options go here.
    },
    your_target: {
      // Target-specific file lists and/or options go here.
    },
  },
})
```

### Options

#### options.transport
Type: `Object`
Default value: `null`

A nodemailer transform object. See the [official documentation](https://github.com/nodemailer/nodemailer#transports) for details.

**Note:** if not set will default to `sendmail` transport.

#### options.message
Type: `Object`
Default value: `{}`

E-mail message configuration. See [Nodemailer documentation](https://github.com/andris9/Nodemailer#e-mail-message-fields) for available options.

#### options.recipients
Type: `Array`
Default value: `[]`

A collection of recipients. Every item should have 2 properties:

* `name`: Name of the recipient
* `email`: E-mail address of the recipient

**Note**: Recipients specified here will be appended to those set in `options.message.to`.

#### options.from **DEPRECATED since v0.2**

See [options.message](#optionsmessage) for details.

#### options.subject **DEPRECATED since v0.2**

See [options.message](#optionsmessage) for details.

#### options.html **DEPRECATED since v0.2**

See [options.message](#optionsmessage) for details.

#### options.text **DEPRECATED since v0.2**

See [options.message](#optionsmessage) for details.

### Using external files


_**BREAKING CHANGES**: before v0.2 you had to provide 2 source files in order to pass HTML and text body message. Since v0.2 text files are automatically discovered. Keep reading for details._

Instead of providing `text` and `html` message options you may use external files by setting a `src` property on the sub-task. All file extensions except `.html` will be used as `text` option for the email.

If HTML files are provided, the task will look for `.txt` files with same filename to be used as text fallbacks.

Example:

```js
nodemailer: {
  test: {
    options: { /* ... transport, message, etc ...*/},
    src: ['email-body.html']
  }
}

//Will use email-body.html for HTML email body,
//optionally using email-body.txt (if found) as text fallback
```

If multiple files are passed in, a message for each one will be delivered.

```js
nodemailer: {
  test: {
    options: { /* ... transport, message, etc ...*/},
    src: ['email-body.html', 'email-body2.html']
  }
}

//Will send 2 messages
```

**Note**: If HTML files include `title` tag it'll be used as the message subject.

### CLI Options

#### --fileSrc
Type: `String`
Default value: `null`

Overrides `src` files set in the task configuration.

### Usage Examples

#### E-mail delivery with Gmail SMTP server

This configurations uses Gmail's SMTP service as transport.

By running the `nodemailer:external` task HTML body will be overridden.

```js

var nodemailer = require('nodemailer');

var gmailTransport = nodemailer.createTransport('smtps://john.doe%40gmail.com:password@smtp.gmail.com');

grunt.initConfig({
  nodemailer: {

    options: {
      transport: gmailTransport,
      message: {
        subject: 'A test e-mail',
        text: 'Plain text message',
        html: '<body><h1>HTML custom message</h1></body>',
      },
      recipients: [
        {
          email: 'jane.doe@gmail.com',
          name: 'Jane Doe'
        }
      ]
    },

    inline: { /* use above options*/ },

    external: {
      src: ['email-body.html']
    }
  }
});
```

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [Grunt](http://gruntjs.com/).

## Release History

1.0.0 Updated to Nodemail 1+ APIs, added support for every filetype as external source (thanks to @wardpeet)

0.3.0 Added grunt cli option to override file src (thanks to @posabsolute)

0.2.1 Display error message instead of error name (fixes issue #3 thanks to @aszmyd)

0.2.0 - Task allows for multiple external sources to better comply to [Grunt's file APIs](http://gruntjs.com/configuring-tasks#files). As of this version, you may set any supported [Nodemailer message's option](https://github.com/andris9/Nodemailer#e-mail-message-fields) onto `options.message`. Added some tests.

0.1.2 - Replaced [deprecated](http://gruntjs.com/blog/2013-11-21-grunt-0.4.2-released) reference to `grunt.util._` with `lodash` npm module

0.1.1 - Bugfix

0.1.0 - Initial release
