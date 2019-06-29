# Hello, Dataset


Ok, we're going to start simple. create a new file called `hello.csv`, paste in the following contents and hit save:

```
letter,number,color
a,0,red
b,1,orange
c,2,yellow
d,3,,
```

Cool. Now let's make a dataset with the `save` command. `cd` to the same directory as `hello.csv` and run the following:

```
$ cd /path/to/workshop/dir
$ qri save --body hello.csv me/hello
```

You should get output that looks _kinda_ like this. Hashes will be different:

```
dataset saved: b5/hello@QmRqNYdBQ5wtimCwBg1KuGBNHnwR4t2idzXkYv7UGw6cCx/ipfs/QmY9h9SKEqhoiAvY3Qirxt2FxEx3eD6B8QWFXUk1Lyoz16
this dataset has 1 validation errors
```

Bueno. We now have a new dataset with a single version. Let's unpack what just happened.

`save` is the way we create a new version of a dataset. `save` is qri's word for `git commit`. Qri has no staging area, our dataset went straight into version control without the need to "add" first. 

The `--body` flag told qri the _body_ component of the dataset is a csv file. We call the "data" part of a dataset the "body", borrowing from the html `<body>` tag. There are other components of a datset we'll learn about, all other components are _about the body_. Qri is sensitive to file extensions, and can store datasets in `csv`, `json`, `cbor`, and `xlsx` formats.

That `me/hello` part is a dataset _name_. Names are a reference to the most recent version of a dataset. A dataset name is your peername `/` a string to defined the dataset . In qri we can use the keyword "me" as a standin for our peername.

You may notice we didn't provide a "commit message" to this version. In qri commit messages are optional. qri will create a summary of changes for you if you don't provide a message.

We'll get into more detail about the "IPFS bits" of qri in a second, so for now we'll skip to that validation error.

Let's go back to our `hello.csv` file & make some changes. here we'll drop that comma and add `green` to the fourth row. We can happily edit this file because it's in version control.

```
letter,number,color
a,0,red
b,1,orange
c,2,yellow
d,3,green
```

With those small changes saved, we re-run the same save command:

```
$ qri save --body hello.csv me/hello
```

passing the same name will add a new version to our dataset history. we can check our history with the `log` command to see our two commits:

```
$ qri log me/hello
```

At any time you can press `q` on your keyboard to get back to the terminal prompt.


Congrats. You've created a versioned, cleaned dataset!