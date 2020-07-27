## Build the site and make it available on a local server.
```sh
bundle exec jekyll serve
```

## Rebuild your Jekyll site when a file changes with the --watch switch

### Install
```sh
gem install jekyll-watch
```

### Usage
Pass the --watch flag to jekyll build or jekyll serve:

```sh
jekyll build --watch
jekyll serve --watch # this flag is the default, so no need to specify it here for the 'serve' command
```

## Browse to http://localhost:4000

## bundle exec just-the-docs rake search:init
```sh
bundle exec just-the-docs rake search:init
```

## Offline support

1. On your development machine, cache the gems required by your applications into vendor/cache directory:

```sh
bundle package --all --all-platforms
```

This will package all required gems into *vendor/cache* folder, for all platforms (that ensures that using different platforms for development and deployment won’t be an issue).

Remember to run that command any time you update dependencies.

2. Make sure vendor/cache is checked into source control so that the offline machine can use it to install the gems.

3. Once deployed to the node without internet, run
```sh
bundle install --local
```

### Reference

[Getting-started](https://pmarsceill.github.io/just-the-docs/#getting-started)

[Github-link](https://github.com/pmarsceill/just-the-docs)