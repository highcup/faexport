FAExport
========

Simple data export and feeds from FA.
Check out the [documentation](http://faexport.boothale.net/docs) for a full list of functionality.
The file `lib/faexport/scraper.rb` contains all the code required to access data from FA.

Development Setup
-----------------

Create a file named `settings.yml` in the root directory containing a valid FA cookie:

~~~yaml
cookie: "b=3a485360-d203-4a38-97e8-4ff7cdfa244c; a=b1b985c4-d73e-492a-a830-ad238a3693ef"
~~~

This cookie can be generated by visiting any FA page and running the folowing code in the developer console:

~~~javascript
document.cookie.split('; ').filter(function(x){return/^[ab]=/.test(x)}).sort().reverse().join('; ')
~~~

If you're having trouble with this, check out [issue 17](https://github.com/boothale/faexport/issues/17).

You may want to do this in a private browsing session as logging out of your account will invalidate
the cookie and break the scraper.

Install [Redis](http://redis.io/), [Ruby](https://www.ruby-lang.org/) and [Bundler](http://bundler.io/),
run `bundle install` then `bundle exec rackup config.ru` to get a development server running.
For example, on a Debian based system you would run:

~~~text
sudo apt-get install redis-server ruby ruby-dev
sudo gem install bundler
bundle install
bundle exec rackup config.ru
~~~

Deploying - Docker
---------
This application is available as a docker image, so that you don't need to install ruby, and bundler and packages and such.
The docker image is available on docker hub here:
https://hub.docker.com/r/deerspangle/furaffinity-api

You can pull the docker image with this command:
```shell
docker pull deerspangle/furaffinity-api
```

And then run the image like so, specifying your FA cookie in the environment variable passed into the image.
```shell script
docker run \
-e FA_COOKIE="b=..; a=.." \
-p 80:9292 \
--name fa_api
deerspangle/furaffinity-api
```

Internal to the docker image, the API exposes port 9292, you can forward that to whichever port you want outside with the `-p` option, in the case above, we're forwarding port 80 into it.


Deploying - Heroku
---------

This application can be run on Heroku, just add an instance of 'Redis To Go' for caching.
Rather than uploading `settings.yml`, set the environment variable `FA_COOKIE`
to the generated cookie you gathered from FA.
