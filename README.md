# DataDog Synthetics Action

This is the GitHub Action wrapper for the DataDog CI synthetics command.<br>
https://github.com/DataDog/datadog-ci/tree/master/src/commands/synthetics

This action by default is a composite action that installs `@datadog/datadog-ci` for your workflow as well as runs the command with the appropriate arguments. You are able to just use the JavaScript action, but you will have to install the dependency yourself.

## Inputs

All inputs for the action are not required but the ones that are marked are required for the CI command. You will be able to set the required inputs through environment variables instead of passing it into the action.

| Input                | Type    | Required           | Default           | Description |
| :------------------- | :-----: | :----------------: | :---------------: | :----------------------------------------------------------------------------------------------------------------- |
| apiKey               | String  | :white_check_mark: | ''                | DataDog API key which can also be set by setting `DATADOG_API_KEY` as an environment variable                      |
| appKey               | String  | :white_check_mark: | ''                | Datadog app key which can also be set by setting `DATADOG_APP_KEY` as an environment variable                      |
| configPath           | String  |                    | 'datadog-ci.json' | DataDog Synthetics CI configuration file path                                                                      |
| datadogSite          | String  |                    | 'datadoghq.com'   | DataDog Host Site which can also be set by setting `DATADOG_SITE` as an environment variable                       |
| failOnCriticalErrors | Boolean |                    | False             | Set run to fail on critical errors                                                                                 |
| failOnTimeout        | Boolean |                    | True              | Set run to fail on timeout                                                                                         |
| files                | Array   |                    | ''                | Synthetic test run configuration files that is set as a multiline input                                            |
| publicIds            | Array   |                    | ''                | Synthetic test public IDs that is set as a multiline input                                                         |
| search               | String  |                    | ''                | Synthetic test search query which is comma separated (Ex: 'tag:e2e-tests, env:prod')                               |
| subdomain            | String  |                    | 'app'             | Custom subdomain to access DataDog which can also be set by setting `DATADOG_SUBDOMAIN` as an environment variable |

## Environment Variables
https://github.com/DataDog/datadog-ci/tree/master/src/commands/synthetics#setup

- **DATADOG_API_KEY**
- **DATADOG_APP_KEY**
- **DATADOG_SITE**
- **DATADOG_SUBDOMAIN**
- **DATADOG_SYNTHETICS_LOCATIONS**

## Configuration File

You can setup a configuration file `datadog-ci.json` or another filename, where the file path will need to be set to the `configPath` input of the action.<br>
https://github.com/DataDog/datadog-ci/tree/master/src/commands/synthetics#api

## Example Usage

```yml
uses: OpenActions/dd-synthetics@v1.0.4
with:
  apiKey: ${{ secrets.DD_API_KEY }}
  appKey: ${{ secrets.DD_APP_KEY }}
  search: 'tag:e2e-tests'
```

## Using JavaScript Action

You can also just use the JavaScript action by itself without the main action. However, you will need to install `@datadog/datadog-ci` NPM package globally in your workflow.

```yml
steps:
  - name: Install DataDog CI
    run: npm i -g @datadog/datadog-ci
    
  - uses: OpenActions/dd-synthetics/js@v1.0.4
    with:
      apiKey: ${{ secrets.DD_API_KEY }}
      appKey: ${{ secrets.DD_APP_KEY }}
      search: 'tag:e2e-tests'
```

## Test Locally

Run test from the root directory and not the `js` folder as we do not want to increase the `node_modules` size for the JS action.

```sh
npm run test
```

## Test Action

Run the `composite.yml` or `javascript.yml` workflow with the `workflow_dispatch` set to the branch you are working on. Both of these workflows will run the local version of the action. Currently the action will fail locally due to the repository not having a valid DataDog credential. You want to see that the command runs properly and the error is handled appropriately.

## Release

Create a release from GitHub and follow SemVer to set the version of the action. When you create a release both the composite and JavaScript action will be deployed.
