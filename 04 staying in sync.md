# Staying in Sync

We'll end today's workshop with a whirlwind tour through some of qri's more advanced features. We're going to build a very small dataset that does one thing: report the latest release of [js-ipfs](https://github.com/ipfs/js-ipfs). It's a hassle to constantly check for the latest release, so we're going to automate qri to do the checking for us. This is a classic example of syncing a dataset to an external source.

### Starlark

Qri includes a way to bind code to datasets using an embedded language called _starlark_. Starlark is a turing incomplete, python-like langauge we've built into Qri. Qri runs starlark in a sandbox, with  access to a [standard libarary](http://qri.io/docs/reference/starlib/) of tools . We can use starlark to create _transform scripts_ that automates part or all of creating a new version of a dataset.

Here we'll write a small transform script that uses the github API to set the body of our dataset to the latest release. First, create a new file called `download.star` with the following contents:

```python
load("http.star", "http")

def download(ctx):
  res = http.get("https://api.github.com/repos/ipfs/js-ipfs/releases")
  return res.json()[0]

def transform(ds, ctx):
  ds.set_body(ctx.download)
```

`download` and `transform` are special functions called by qri.

And just for a bit of fun, let's add a viz template to this dataset before we save it. Create a new file called `template.html` with the following contents:

```html
{{ $release := allBodyEntries }}
<!DOCTYPE html>
<html>
  <head>
    <title>{{ title }}</title>
  </head>
  <body style="background: #041C2D; color: #6ACAD1; font-family: Helvetica; font-size: 150%">
    <div style="margin: 3em auto; padding: 2em; width: 600px;">
      <h4>The Latest JS IPFS release is:</h4>
      <h1 style="font-size: 5em">{{ $release.tag_name }}</h1>
      <small>published: {{ $release.published_at }}</small>
      <br />
      <small>last change found: {{ ds.commit.timestamp }}</small>
    </div>
  </body>
</html>
```

Here we're providing qri with an HTML document to visualize the current version.

Now we'll save that dataset, supplying both the viz and download script as `--file` flags to a dataset named `js_ipfs_latest_release`:

```
qri save --file download.star --file template.html me/js_ipfs_latest_release
```

congrats, you've just written and used your first transform script!

When you provided a transform script to Qri, it embedded the script into the dataset, which means Qri now has a script in it's history we're able to access & reuse to make the dataset self-updating. This script also forms part of the _audit trail_ of your dataset, making it possible for others to understand exactly what steps you took to get from one transform state to the other.

### Scheduled updates

But we have another problem. We don't know _when_ the next release of js-ipfs is coming out. We know the next release will be _soon_, but soon means we'll have to constantly check the js-ipfs repo, waiting for a release.

Qri includes an _update daemon_ that allows us to schedule checks for fresh data. We can start the daemon and _schedule_ our dataset to re-run the transform script at some time interval. On every interval occurance, the update daemon will start qri, run the transform script, and compare the results to existing dataset version. If nothing has changed, qri will record a record of having checked, but _not_ create a new version. If qri detects new data, it'll automatically update my dataset for me.


First, we need to start the update daemon. note that the `--daemonize` flag only works on mac os x.

```
qri update service start --daemonize
```

This _doesn't_ start IPFS in the background, only a very small process that starts cron jobs. Next we tell qri to run periodical checks for changes with `update schedule`. For demo purposes we'll choose a very short interval of one check per minute:

```
qri update schedule me/js_ipfs_latest_release R/PT1M
```

We can see our newly scheduled update with `update list`:
```
qri update list
```

then we relax for a minute or two. After a while we can run `qri update log`:

```
qri update log
```

And since js-ipfs (probably) didn't release a new version in the last minute, we'll see `no changes` detected in the log. Qri logs

Then finally, let's unschedule this rediculousness with `update unschedule`:

```
qri update unschedule [peername]/js_ipfs_latest_release
```

> bug alert! currently unschedule doesn't work with 'me'. I've filed an [issue](https://github.com/qri-io/qri/issues/817)

Slammin. If you're not going to run any other qri updates, it makes sense to turn the update daemon off entirely:

```
qri update service stop
```

-- --

This concludes a top-line tour of qri's features. There are a bunch we didn't cover today like
diffing; scheduling shell scripts; publishing; remotes; search; transform dependency graphs; schema design; xlsx integration; our electron app; and moar! If you'd like to learn about any of this, or how qri could work for you, please check out our docs, and feel free to [join our discord](https://discordapp.com/invite/etap8Gb).

Thanks!