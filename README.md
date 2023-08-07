## Setup

- npm package with two [workspaces](https://docs.npmjs.com/cli/v7/using-npm/workspaces) `client` and `universal`.
  `client` has a dependency on `universal` in its `package.json`.
- [volta](https://volta.sh/) is configured in root `package.json`.
  workspaces reference root volta config.
- [lerna](https://lerna.js.org/) installed

## How to reproduce

- Run `npm ci`
- Run `npm run dev` repeatedly.
  `npm run dev` uses lerna to run the `dev` script in each of the workspaces.
  The `dev` scripts just print the packages name and working directory. 

### Actual output

After several times (sometimes even right after `npm ci`), the output is as follows.
The `dev` script of one workspace is run twice, but in both workspace folders.
This is not supposed to happen.

```
package universal started in /Users/simon/test/appshell-dev/packages/client
package universal started in /Users/simon/test/appshell-dev/packages/universal
```
or equivalently
```
package client started in /Users/simon/test/appshell-dev/packages/universal
package client started in /Users/simon/test/appshell-dev/packages/client
```

### Expected output

On every run, the `dev` script of each package should be run in the correct workspace.
```
package universal started in /Users/simon/test/appshell-dev/packages/universal
package client started in /Users/simon/test/appshell-dev/packages/client
```

## Observations

- Without volta, the `dev` scripts are run correctly every time.
- After `npm ci`, the scripts are run correctly for a few times. 
