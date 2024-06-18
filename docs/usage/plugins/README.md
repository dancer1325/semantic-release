* := npm module /
  * can implement >= 1 release steps 
* Release steps
  * Check '.../README.md' for the release steps
  * 👁️ / release step, if plugin implements the step -> `semantic-release` runs the plugin 👁️
    * if NO plugin implements `analyzeCommits` -> `@semantic-release/commit-analyzer` plugin is run
      * Reason: 🧠 `analyzeCommits` is mandatory 🧠 
* Check '../extending/plugins'

## Examples
* Identify the release steps executions for the next plugins configuration
  ```
  {
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "@semantic-release/npm",
    "@semantic-release/git"
  ]
  }
  ```
  * `verifyConditions` step must be executed -> it's executed
    * `verifyConditions` implementation in `@semantic-release/npm` & then  
    * `verifyConditions` implementation in `@semantic-release/git`
  * `analyzeCommits` step must be executed -> it's executed
    * `analyzeCommits` implementation in `@semantic-release/commit-analyzer`
  * `generateNotes` step must be executed -> it's executed
    * `generateNotes` implementation in `@semantic-release/release-notes-generator` 
  * `prepare` step must be executed -> it's executed
    * `prepare` implementation in `@semantic-release/npm` & then
    * `prepare` implementation in `@semantic-release/git`
  * `publish` step must be executed -> it's executed
    * `publish` implementation in `@semantic-release/npm`