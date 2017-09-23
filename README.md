Forked from [Advanced-WordPress-on-Pantheon](https://github.com/ataylorme/Advanced-WordPress-on-Pantheon) - refer there for more docs. Stripping this down to just setup / config.

## Circle CI Setup
You will need to add the following environment variables in the Circle CI UI. See [https://circleci.com/docs/2.0/environment-variables](https://circleci.com/docs/2.0/environment-variables/)/ for details.

* TERMINUS_SITE:  Name of the Pantheon site to run tests on, e.g. my_site
* TERMINUS_TOKEN: The Pantheon machine token
* GITHUB_TOKEN:   The GitHub personal access token
* GIT_EMAIL:      The email address to use when making commits
* TEST_SITE_NAME: The name of the test site to provide when installing.
* ADMIN_PASSWORD: The admin password to use when installing.
* ADMIN_EMAIL:    The email address to give the admin when installing.

You'll also need to set up an SSH key for deployment to Pantheon. 

#### Add Private SSH key to Circle CI
* Copy your current private key `$pbcopy < ~/.ssh/id_rsa`
* Add to CircleCI at Project > Permissions > SSH Permissions. You can leave `hostname` blank to use for all builds.

#### Add Public SSH key to Pantheon
* Copy your current public key `$pbcopy < ~/.ssh/id_rsa.pub`
* Add to Pantheon under Account > SSH Keys

Once this is in place, the `build_and_deploy` step should pass on Circle. 

## Local Setup
In order to develop the site locally a few steps need to be completed. 
These steps only need to be performed once, unless noted. 

* Open a terminal
* Checkout the Git repository
* Enter the Git docroot
* Install Composer if not already installed
* Copy `sample.env` to `.env` and update the values accordingly
* Install Node JS, NPM and Yarn if not already installed
* Run `./bin/local-build.sh` to install Composer dependencies and compile assets with gulp

## Notes
* `npm install` will need to be ran after any changes to `web/wp-content/themes/twentyseventeen-child/package.json` 
* `composer update` will need to be ran after any changes to `composer.json`

### Local Development
The gulp _watch_ task initates a BrowserSync session and watches for:
* Changes to `web/wp-content/themes/twentyseventeen-child/source/css/twentyseventeen-child.scss`, recompiling the CSS build files and injecting changes into the browser
* Changes to `web/wp-content/themes/twentyseventeen-child/source/js/twentyseventeen-child.js`, recompiling the JavaScript build files and reloading the browser
* Changes to `.php` files in the `web/wp-content/themes/twentyseventeen-child` directory, reloading the browser

To start the watch task run `gulp watch` from the `web/wp-content/themes/twentyseventeen-child` directory.
When you are done developing stop the task with `ctrl + c`.
