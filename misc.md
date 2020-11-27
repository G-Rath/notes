# Misc notes that are useful but don't have a better place to live (yet)

## How can I find out what my public IP is in a script?

https://checkip.amazonaws.com/

There are a few other highly stable URLs you can use, but this one can look more
professional since you're hitting AWS instead of a random url in your script.

## How can I get `nano` in a `heroku run bash` session?

So you've just done `heroku run bash` to debug your production app, but find you
can't edit any files as there are no editors installed, and getting them
installed seems to be either really hacky or painful.

So do this instead:

```
mkdir /app/nano
curl https://github.com/Ehryk/heroku-nano/raw/master/heroku-nano-2.5.1/nano.tar.gz --location --silent | tar xz -C /app/nano
export PATH=$PATH:/app/nano
```
