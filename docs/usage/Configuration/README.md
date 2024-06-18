* Configuration of
  * Git repository
    * `repositoryUrl`
      * TODO:
    * `branches`
      * TODO:
    * `tagFormat`
      * TODO:
  * `plugins`
    * returns `[]`
      * default 
        ```
        ['@semantic-release/commit-analyzer', '@semantic-release/release-notes-generator', '@semantic-release/npm', '@semantic-release/github']
        ```
      * if you declare it -> override the default one 
    * `-p` \ `--plugins`
      * for CLI arguments
    * allows
      * defining the list of plugins to use
  * run mode
    * `debug`
      * TODO:
    * `dryRun`
      * TODO:
    * `ci`
      * TODO:
* Ways to configure
  * config file
    * ".releaserc" file
      * format
        * .yaml OR .yml OR
        * .json
        * .js
        * .cjs
        * .mjs
        ```
        {
          "branches": ["master", "next"]
        }
        ```
    * "release.config.(.js | .cjs | .mjs)" / exports an object
        ```.cjs
        /**
        * @type {import('semantic-release').GlobalConfig}
        */
        module.exports = {
          branches: ["master", "next"],
        };
        ```
        ```.mjs
        /**
        * @type {import('semantic-release').GlobalConfig}
        */
        export default {
          branches: ["master", "next"],
        };
        ```
    * "package.json" / `release` key
        ```
        {
          ...
          "release": {
            "branches": ["master", "next"]
          }
          ...
        }
        ```
  * CLI arguments
     ```
     semantic-release --branches next
     ```
    * 👁️ CLI arguments priority > previous files priority 👁️
    * ⚠️NOT possible to define plugin options ⚠️
  * Check '../shareableConfigurations'
* Other options
  * `extends`
    * TODO: