# package-sandbox

A drop-in replacement for NPM's `script-shell` configuration
that sandboxes the dependencies' scripts (e.g. deny network access)
in response to situations like https://twitter.com/o_cee/status/892306836199800836 .

Targeted platforms:

* OSX via [sandbox-exec](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/sandbox-exec.1.html)


## Installation

```shell
npm install --global --ignore-scripts git://github.com/andreineculau/package-sandbox.git
npm config set script-shell "$(which package-sandbox)" --global
```


## Usage

Run your `npm` commands as usual.

By default, `package-sandbox` will deny network access to all `npm` scripts
that are called as part of `npm install`, `npm run`, etc.

If you want to deny more than just network access, use

``` shell
npm config set package-sandbox-path path/to/sandbox-exec-profile          # next to your top-level package.json
cd && npm config set package-sandbox-path path/to/sandbox-exec-profile    # or in your home folder
npm config set package-sandbox-path path/to/sandbox-exec-profile --global # or globally
```

and create your own sandbox-exec profile ([reference](https://reverse.put.as/wp-content/uploads/2011/09/Apple-Sandbox-Guide-v1.0.pdf))
which may deny reading your SSH keys, writing anywhere else but in specific folders, etc.


## Playground

If you want to test, clone this repository and let's sandbox it!

```shell
cd $(mktemp -d)
git clone git://github.com/andreineculau/package-sandbox.git
cd package-sandbox

export PACKAGE_SANDBOX_ROOT=${PWD}
npm run ping-google    # should fail to ping google.com
npm run ping-localhost # should fail to ping localhost

PACKAGE_SANDBOX_TRACE=1 npm install          # install devDependencies with trace on
find node_modules -name "package.*.trace.sb" # list trace files
```

**NOTE** that running with trace on ignores all other rules.

**NOTE** that the trace files include also sh/bash access.


## Status

This is a proof-of-concept.

Contributions are highly appreciated both to the default profile and to the `package-sandbox` script itself.

Supporting other platforms is secondary at this moment, but if you can cook something up - don't hide!


## License

[UNLICENSE](UNLICENSE)
