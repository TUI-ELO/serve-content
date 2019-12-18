# Installing

Assuming you’ve already installed [Node.js](https://nodejs.org/), create a directory to hold your application, and make that your working directory.

```
$ mkdir myapp
$ cd myapp

```


<<<<<<<<<
<Note>
  # This is an interesting note
  
=========
Use the `npm init` command to create a `package.json` file for your application. For more information on how `package.json` works, see [Specifics of npm’s package.json handling](https://docs.npmjs.com/files/package.json).

```
$ npm init

```

This command prompts you for a number of things, such as the name and version of your application. For now, you can simply hit R
>>>>>>>>>
E
<<<<<<<<<
xpress is a useful tool for API developme
=========
TURN to accept the defaults for most of them, with the followi
>>>>>>>>>
ng except
<<<<<<<<<

</Not
=========
ion:

```

>>>>>>>>>
e
<<<<<<<<<
>

Ente
=========
nt
>>>>>>>>>
ry point: (index.js)

```

Enter `app.js`, or whatever you want the name of the main file to be. If you want it to be `index.js`, hit RETURN to accept the suggested default file name.

Now install Express in the `myapp` directory and save it in the dependencies list. For example:

```
$ npm install express --save

```

<ShortExercise id="qF6bVXu181vyZD5saibt" title="Vraag 1">
  Hoe gebruik je express?
</ShortExercise>

To install Express temporarily and not add it to the dependencies list:

```
$ npm install express --no-save

```

By default with version npm 5.0+ npm install adds the module to the `dependencies` list in the `package.json` file; with earlier versions of npm, you must specify the `--save` option explicitly. Then, afterwards, running `npm install` in the app directory will automatically install modules in the dependencies list.
<<<<<<<<<


<ShortExercise id="m2rc4CGvBKZuk7kr2zl0" title="Vraag 2">
  Waarom gebruik je express?
</ShortExercise>

=========

>>>>>>>>>
