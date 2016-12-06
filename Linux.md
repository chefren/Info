# JavaScript
## NPM
- [Tutorial](https://www.sitepoint.com/beginners-guide-node-package-manager/)

### Set global install environment to some local folder (similar to venv)
`cd && mkdir .node_modules_global`

`npm config set prefix=$HOME/.node_modules_global`

This also creates a `.npmrc` file in our home directory. Then install npm in the local path.

`npm install npm --global`
