[![forthebadge](http://forthebadge.com/images/badges/pretty-risque.svg)](http://forthebadge.com)

# Acme Styleguide

This is an example project that uses [hologram](http://trulia.github.io/hologram) to build [this style
guide](http://acme-styleguide.cfapps.io). The instructions listed below assume that you have already installed Node / NPM, and have the Apple Developer Tools installed on your machine.

## Orientation

Hologram is a Ruby gem that parses specially formatted comments in your Sass/CSS and turns them into a living styleguide.

The two main directories that you will be interacting with are `styleguide-theme` and `styles`. 

The former directory is home to the Header and Footer of your styleguide, as well as any static assets you might need to theme it (Yes, you can _style_ the _styleguide_ via this directory). Note that the Header and Footer are simple HTML files, wherein you can pull in 3rd party assets (in this case: [Font Awesome](http://fontawesome.io), and the [Roboto Web Font](https://fonts.google.com/specimen/Roboto)). Hologram is smart enough to spit out the content of your styleguide _in between_ said header and footer, so don't worry about having to know any Ruby!

Where the fun begins is the `styles` folder, which is home to a list of Sass files. To get your project kickstarted, we've added [Bootstrap 4](http://getbootstrap.com) to this directory. In any case, feel free to write vanilla CSS, or SCSS-flavored Sass -- the only requirement is that any new files you add to the directory are imported in the `style.scss` file. For example, if you were to add a new file to the directory called `_toasts.scss`, be sure to add a corresponding statement in `style.scss`.


    @import 'toasts';

In order for that element to be added to the styleguide, add a [Hologram Comment](https://github.com/trulia/hologram#quick-start) to the top of the file.


    /*doc
    ---
    title: Toasts
    name: toast
    category: components
    ---

    Toasts are not only delicious to eat, but they also...

    ```html_example
    <div class="toast">
      <p>I'm a paragraph inside the toast</p>
    </div>
    ```

    */

## Gulp / Automation?

As if we needed another tool, there's another one that you should be aware of: [Gulp](http://gulpjs.com/). We use Gulp to perform a bunch of tedious tasks that would otherwise get in the way of us doing what we do best: designing. Namely, it does the following:

1. Processes and concatenates all of the `.scss` files inside of the `styles` directory and chucks them into a dynamically generated `built-styles` directory.
2. Runs the `hologram` command to create the styleguide as per the brand new stylesheet from Step #1. The output of this step is a brand new `styleguide-dist` directory.
3. Spins up a web server that points to the `styleguide-dist` directory described above.
4. Watches any changes to your `.scss` files and runs steps 1-3 again.

## Installation

### Ruby
Recommend using [RVM](https://rvm.io/rvm/install) so you don't have to `chown` to use your system Ruby. Follow the steps at their site to download and restart your shell session.

### Node / NPM
https://docs.npmjs.com/getting-started/installing-node

### Apple Developer Tools
```
xcode-select --install
```
### Install Dependencies / Spin up Server

    npm install -g gulp-cli
    npm install
    gem install bundler
    bundle install
    gulp

A web browser should open at [http://localhost:8080](http://localhost:8080)  




## Deploying your styleguide

### PWS
1. Download the [CF CLI](https://github.com/cloudfoundry/cli#downloads)  
2. `$ cf login`  
3. `$ cf create-space [spacename]` *[i]*  
4. `$ cf target -o "[orgname]" -s "[spacename]"` *[i]*  
5. `$ cf push` to deploy your styleguide using the staticfile buildpack using the [manifest](https://github.com/mattrothenberg/styleguide-boilerplate/blob/master/manifest.yml)  
*[i] This can also be managed online via [PWS](http://run.pivotal.io)*

### Heroku

Make sure you have the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-command-line) downloaded. Then login to heroku and install the static website cli:

    heroku login
    heroku plugins:install heroku-cli-static

Create a new Heroku site, and set the buildpack config

    heroku create
    heroku buildpacks:set https://github.com/hone/heroku-buildpack-static

Initialize the static directory. In our case it's the `styleguide-dist` folder.

    heroku static:init

You should ECHO out something like this:

```
? Enter the directory of your app: styleguide-dist
? Drop `.html` extensions from urls? No
? Path to custom error page from root directory:
{
  "root": "styleguide-dist",
  "clean_urls": false
}
```

Now deploy! Scale up a web server, open & enjoy :)

    heroku static:deploy
    heroku ps:scale web=1
    heroku open

Here's our same styleguide, running on Heroku => https://arcane-tundra-22168.herokuapp.com/
