## What's included

### configs &amp; dotfiles

**Dotenv**
`.env`
[dotenv](https://www.npmjs.com/package/dotenv) is a node module to [manage environment variables](https://12factor.net/config) in `.env` files. **You won't have a .env file in this repo** because it never gets checked in. Currently it's being used to store S3 credentials for deploying to the CDN. I'll set you up.

**.npmrc**
The `@exa` modules are private npm repos. This config file let's you npm install them without being a member of the `@exa` org.

**Dockerfile**
This container includes node, puppeteer and a few key global npm packages (eg. node-sass).

**.stylelintrc**
Sass linting config. See config options [here](https://stylelint.io/)

**.babelrc**
Ready by `gulp-babel` and used to convert es6 .js to a legacy .es5 format for ie11.

> Note: there's no .js linting r/n. I vaguely remember standard.js being a blocker on a recent program.

**gulpfile.js**
The root gulpfile uses [require-dir](https://www.npmjs.com/package/require-dir) to require the entire `gulp` directory. This helps separate the gulpfiles by concern (eg. js, sass, deploy). See the **Tasks** section.

**package.json**
I've added a custom `exa` property where I'm storing cdn endpoints.

## sass

**Key takeaways**
* sass follows this [module definition technique](http://thesassway.com/intermediate/a-standard-module-definition-for-sass)
* css rules are written with [bem](http://getbem.com/introduction/) syntax.

### Different types of sass files

**component.rules.scss**  
All of your component's css gets written in the mixin (`component-rules()`) included in this file.

**component.variables.scss**
All the vars for your component go here.

**import.scss**
Imports all the sibling `scss` files EXCEPT for `output` (see below).

**output.scss**
Outputs all the css rules for the component. Used by `gulp` to generate the css file in `dist`.

#### The difference between `import` and `output`

* `import` is a convenience to grab up all the `scss` in a project or directory. In projects with multiple `scss` directories I include an `import` in each folder and then a root `import` to include them all.  
* Importing `import` doesn't output any css into your project. This may be beneficial for authors who want to wrap a component's css rules in a special class or organize them in a particular way. **Note** that importing `import` will include any functions and vars into your project.
* `import` doesn't import any dependencies (eg. `@exa/common`).
* `output` imports all dependencies and includes the mixin for your main rules module with all your css.

#### Not included in this project
**component.mixins.scss**
If your component introduces new mixins.

**function-name.function.scss**  
If your component introduces new functions.

### Naming sass vars

I tried to start with [bem](http://getbem.com/introduction/) as a model.

```scss
$color__text : #222;
$color__text--muted : #666;
```
