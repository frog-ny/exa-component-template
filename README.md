# Prerequisites [docker](https://store.docker.com/editions/community/docker-ce-desktop-mac)

# Installation
#### Build the docker image and container
```
npm run docker
```

# Development
#### Start the docker container
```
npm start
```

#### Exit the docker container
```
exit
```

#### Build and Deploy to AWS inside docker container
```
npm run deploy
```


# Notes

## configs &amp; dotfiles

**Dotenv**
`.aws`
for AWS deployment, will auto generate when you run 'npm run deploy'

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

---

# sass

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

> **Note** sass var naming isn't resolved. Need some help here.

**Naming objectives**
* Names should indicate the type of variable. Example `$color__text`;
* Names should indicate whether or not the var is local (component-level) or at the system level. Example `$system__font` vs `$dialog__font`;
* Like [bem](http://getbem.com/introduction/) it would be nice to have a naming convention that allows for calling out variations. Example `$color__text--disabled`

**Where naming becomes difficult**
Typing vars was easy...

```sass
$color__text;
```

Declaring local vars was easy...

```sass
$dialog__shadow;
```

Setting modifiers was easy...

```sass
$dialog__text--disabled;
```

...but how do you declare a type on a component-level var without the convention getting clunky.

```sass
$dialog__color__text--disabled;
```

...so now what? Need other ideas.

---

# js

The `src/foo.element.js` is a custom element. It follows the standard spec for setting up an element along with some added functionality to show how a basic counter component would work.

## Custom Elements

Custom Elements info from [MDN](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_custom_elements)

Custom Elements [lifecycle callbacks](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_custom_elements#Using_the_lifecycle_callbacks)

Custom Elements info from [Google](https://developers.google.com/web/fundamentals/web-components/customelements)

[Custom Elements Everywhere](https://custom-elements-everywhere.com/) tracks compatibility with .js frameworks along with issues. They have a github repo that shows their testing criteria.

Custom Elements [caniuse](https://caniuse.com/#search=custom%20elements) **Note** on caniuse that there is a v0 and v1 spec for custom elements. We're using v1.

Also, the support issues differentiate between **Autonomous Custom Elements** and **Customized built-in elements**. **Autonomous** means you're defining a new tag, **Customized** means you're extending the capabilities of an existing tag (eg. button or img). **Customized** isn't widely supported and we should avoid using it.

## js filename conventions

`exa-foo.element.js` - describes a single custom element. The first part of the filename should reflect the new tag being introduced. These files are native, web-standards .js that can be used with supporting browsers. They can be delivered via CDN for use on websites and with tools like codepen.

`exa-foo.element.jsm` - describes a [js module](https://developers.google.com/web/fundamentals/primers/modules). The file is generated by a gulp process that adds an exports statement to the file. These files are helpful when working with .js frameworks that need to import and render your component.

`exa-foo.legacy.js` - describes an element file that's been transpiled down to es5 via `gulp-babel`. **Experimental** - this format needs more testing but will be important for clients using older software.
