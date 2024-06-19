* 👁️Main decision to take about the plugin 👁️
  * release steps / implement !!
    * `verifyConditions` is recommended to implement always 
* How to write a plugin?
  * "main" file (_Example:_ "index.js") /
    * exports an object with functions named as release steps
      ```javascript
        ...
         async function verifyConditions(pluginConfig, context) {
           await verify(pluginConfig, context);
         }
         ...
         module.exports = { verifyConditions };
      ```
      * available functions' arguments
        * `pluginConfig`
          * := object / options -- can be -- passed 
        * `context`
          * provided by `semantic-release`
            * _Example:_ environment variables
* TODO:
* `context`
  * common ones
    * `stdout`
    * `stderr`
    * `logger`
  * / release step
    * TODO:
    * `verifyRelease`
      * Check common ones
      * .
        ```
        nextRelease {
          type: String,
          channel: String,
          gitHead: String,
          version: String,
          gitTag: String,         
          name: String
        }
        ```
* TODO: