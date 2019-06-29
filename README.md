# IPFS Camp Elective course G

:wave: Welcome to elective course G! Today we're going to do data on the d.web. It's going to be sweet.

### Before we begin:
Turn off any running copies of IPFS (including browser extensions like IPFS companion).

Note: This demo will alter the contents of your IPFS repo. If you'd like to do this on a fresh repository, qri always respects the `$IPFS_PATH` environment variable. Just be sure to set it in _every_ terminal you're interacting with qri in. Qri doesn't do anything weird or scary to IPFS aside from pinning stuff. You only need to mess with `IPFS_PATH` if you'really concerned about keeping your repo unaltered.


### Installation

To get started make sure you've downloaded the latest qri CLI for your platform:

https://github.com/qri-io/qri/releases/latest

To get started, place the `qri` binary onto your `$PATH`. Most folks install to `/usr/local/bin`. Open a new terminal & type the following:

```sh
sudo mv qri /usr/local/bin/qri
```

Next, confirm qri is installed by opening a new terminal & typing `qri`. you should see help instructions that start like this:

```
$ qri

qri ("query") is a global dataset version control system
on the distributed web.

...
```

### Setup & Peername

Next, we'll need to set qri up. We do that with the `setup` command:

```
$ qri setup
```

Setup will prompt you to choose a peername, supplying you with a random color-dog combination if you're out of inspiration for a cool handle. Type the peername you'd like & hit enter.

If you'd ever need to remove Qri from your computer, run `qri setup --remove`.

For the workshop create a folder to work from while we do the demo, you'll be be creating & editing text files in this folder, so have your text editor of choice on hand. I'll be working from a folder called `workshop`. Very original, I know :)

That's it! you're now ready for the workshop.

While you're waiting for others to get set up feel free to browse our [docs](https://qri.io/docs) to get a sense of what's to come.