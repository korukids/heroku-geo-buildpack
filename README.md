**NOTE: This repository is no longer supported or updated by Koru Kids. See [heroku-buildpack-apt](https://elements.heroku.com/buildpacks/heroku/heroku-buildpack-apt) instead.**

Heroku buildpack: geo
=====================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) that
vendors main geo/gis libraries like geos, proj and gdal.

You will use this buildpack with other major buildpack such as Ruby buildpack.

Usage
-----

Example usage:

```
$ heroku buildpacks:set https://github.com/cyberdelia/heroku-geo-buildpack.git
$ heroku buildpacks:add heroku/ruby
```

Run `heroku buildpacks` to make sure that `heroku-geo-buildpack` is added before
the language buildpacks.

```
$ heroku buildpacks
=== sushi Buildpack URLs
1. https://github.com/cyberdelia/heroku-geo-buildpack.git
2. heroku/ruby
```

Updating
--------

Binaries for `geos`, `gdal` and `proj` are build on the appropriate heroku stack
image using Docker & pushed to a public S3 bucket.

To update or rebuild these:

* Set the versions and stack environment in `support/docker-compose.yml`
* Set the AWS keys to ones with permission to push to the selected S3 bucket
* If updating the stack, also update it in `support/Dockerfile`
* *Make sure you've deleted any cached heroku docker images*
* Build with `cd support && docker-compose run geo-build`
* Wait 20 minutes, and check the contents of the relevant stack folder on S3

Testing
-------

For Geo Django:

```python
>>> from django.contrib.gis import gdal
>>> gdal.HAS_GDAL
True
```

For rgeo:

```ruby
>>> require 'rgeo'
>>> RGeo::CoordSys::Proj4.supported?
=> true
>>> RGeo::Geos.supported?
=> true
```
