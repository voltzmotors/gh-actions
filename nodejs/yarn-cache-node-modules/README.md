# yarn-cache-node-modules

## Description

This composite action uses the actions/cache@v2 official cache action to cache the `node_modules` folder. 

They cache key is created by composing the name of the github repository that called the action, runner OS and a value passed in the `hash` argument to the action. This value should be a hash (or checksum) of your yarn.lock file. By doing this we will ignore the cache whenever your lock file has changes - which means that dependencies were added, removed or updated. 

The name of the repository isnt a required variable but if you want to pass a different value, you can.


The key will usually look like this: `voltzmotors/partners-Linux-yarn-cache-node-modules-98c9e7f0b8a35c15baeb8be76263c57e62a2efea962f9282e7f085c81d359b6f`

## How to use

Inside a `steps` block, call the composite action by using the `uses` command.

I.e:

```yml
jobs: 
  setup:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Node.js
        uses: actions/setup-node@v2
        with: 
          node-version: '14'
          cache: 'yarn'
      
      # Uses the composite action
      - name: yarn-cache-node-modules
        uses: voltzmotors/gh-actions/nodejs/yarn-cache-node-modules@<version> # <--- change the version
        with: # <--- provide the variables in this block
          runner_os: ${{ runner.os }}
          hash: ${{ hashFiles('**/yarn.lock') }}
      
      - name: build
        run: yarn build
```


### Variables

You must provide two variables: 

- runner.os = the runner operational system (linux, macos and windows)
- hash = the output of the function hashFiles, which consist of the yarn.lock hashed value

It isn't necessary to change this values, except if your `yarn.lock` file is in a different place or if you really need to pass a diferent path to it.

### Version

You must specify the version that your action is going to use. We're using tags, so just mention a specific release after the `@`.

I.e:

```yml
uses: voltzmotors/gh-actions/nodejs/yarn-cache-node-modules@v1.2.3
```
