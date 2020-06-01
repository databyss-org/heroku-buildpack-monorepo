Heroku Buildpack for monorepos
=====================================

This buildpack is useful for monorepos that contain multiple build targets which require different buildpacks on Heroku (i.e. they are bound for different Heroku apps). 

Based on the "outer" buildpack for [create-react-app-buildpack](https://github.com/mars/create-react-app-buildpack).

## Usage

### Package configuration
If `packages/client` uses `create-react-app-buildpack` and `packages/api` uses `heroku-buildpack-nodejs`, you would create a `.buildpacks` files for each of these directories.

In `packages/client/.buildpacks`:
```
https://github.com/heroku/heroku-buildpack-nodejs.git
https://github.com/mars/create-react-app-inner-buildpack.git#v9.0.0
https://github.com/heroku/heroku-buildpack-static.git
```
(these are taken from the `.buildpacks` in [create-react-app-buildpack](https://github.com/mars/create-react-app-buildpack/blob/master/.buildpacks))

In `packages/api/.buildpacks`:
```
https://github.com/heroku/heroku-buildpack-nodejs.git
```

### Environment configuration (Heroku config vars)

Then, in the Heroku configuration for each app, you tell it where to look for the `.buildpacks` file by setting the `APP_BUILD_ROOT` environment variable.

So for the `client` app, you would add the following config var:
```
APP_BUILD_ROOT=packages/client
```

And for the `api` app, you would add:
```
APP_BUILD_ROOT=packages/api
```