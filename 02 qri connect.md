# Understanding Qri Connect

Next let's dig into relationship between Qri & IPFS. While we're at it, we'll give Qri a chance to make sure we're up to speed with the latest webapp. In a new terminal, type the following command and hit enter:

`qri connect`

```
starting IPFS HTTP API:
API server listening on /ip4/127.0.0.1/tcp/5001

📡  Success! You are now connected to the d.web. Here's your connection details:

peername:	b5
profileID:	QmbG9mRR2JxaZ7aXX8k4q7cw4GLdtYdRFxbtirHbfTEDom}}
API port:	2503
RPC port:	2504
Webapp port:	2505
IPFS Addresses:
  /ip4/127.0.0.1/tcp/4001/ipfs/QmUnjJ2iDpHG1QvgiV58LFDU6KCiNeNtf619bSDBmHzS9D
  /ip4/192.168.1.6/tcp/4001/ipfs/QmUnjJ2iDpHG1QvgiV58LFDU6KCiNeNtf619bSDBmHzS9D
  /ip4/10.137.0.214/tcp/4001/ipfs/QmUnjJ2iDpHG1QvgiV58LFDU6KCiNeNtf619bSDBmHzS9D
  /ip6/::1/tcp/4001/ipfs/QmUnjJ2iDpHG1QvgiV58LFDU6KCiNeNtf619bSDBmHzS9D

18:47:34.924  INFO     qriapi: updating webapp to version: /ipfs/QmPXeMgRHLyvnmzex9bdyWgwFd6KwS5dfk62MAoe2QAKZu webapp.go:81
```

`qri connect` does three things for you at the same time: 
* connects you to the distributed web
* sets up a local API server
* serves the Qri webapp

All of these are configurable, and we can see all three are enabled with the section that lists `API port: ...`. To view the webapp open your browser to `http://localhost:2505`. If this it is the first time you've opened Qri, There's a good chance your browser will show a loading from the dweb screen. What gives?

### The journey of the webapp
The qri webapp is automatically shared and updated through IPFS. IPFS is a _peer-2-peer file system_, meaning the code to run the webapp is actually going to come from peers on the distributed web. So to get the webapp, we first need some peers. Let's explore qri's `peer` commands to check in on the webapp's progress.

    > How does Qri know which version of the webapp is current?
    Qri uses IPNS dnslink records to advertise the latest webapp hash

    > What if my computer is completely cut off from all DNS servers?
    Every version of qri is distributed with the latest version of the webapp specified at time-of-publication, so you'd still be able to fetch the webapp from the dweb, just not the latest version.

Leave `qri connect` running in the current terminal, and open a new one. In this new terminal, let's try a new command:

```
qri peers list
```

You should see something like this:

```
1   venda | online
    Profile ID: QmY1weoMk8u5vqECxYv9VD6VHiX5HpekeC2tP4fm9ZMP8W
    Address:    /ip4/35.226.44.58/tcp/4001/ipfs/QmQS2ryqZrjJtPKDy9VTkdPwdUSpTi1TdpGUaqAVwfxcNh

2   blue | online
    Profile ID: QmR1CDBZMAq7EQtqC3wKhiKpnxX9HAdSXXE83hiuTyccra
    Address:    /ip4/35.238.105.35/tcp/4001/ipfs/Qmc353gHY5Wx5iHKHPYj3QDqHP4hVA1MpoSsT6hwSyVx3r

3   yeonis | online
    Profile ID: Qma6cUWKXURHHPHqJLZ3HMbEGTsC5J42xWCiSq4ZfH58fE
    Address:    /ip4/35.202.155.225/tcp/4001/ipfs/QmegNYmwHUQFc3v3eemsYUVf3WiSg4RcMrh3hovA5LncJ2

4   indigo | online
    Profile ID: QmUzFsRhru6bGr5uNJiGp1uW6TaVpKYKZ3oubf1J6Vggm1
    Address:    /ip4/35.239.138.186/tcp/4001/ipfs/QmT9YHJF2YkysLqWhhiVTL5526VFtavic3bVueF9rCsjVi

(END)
```

At any time you can press `q` on your keyboard to get back to the terminal prompt.

These are your _qri_ peers. Qri peers have a "profileID" that represents a user profile, which is a different thing from your peerID.

Most Qri peers are _also_ IPFS peers, but not all IPFS peers are Qri peers, if that makes sense. The point is, it's a both-and, not an either-or. We can see our IPFS peers with the `network` flag:

```
qri peers list --network=ipfs
```

```
1   QmWuMVrkaMiWaYhnQRK7N4TZCxFTrCuEbjpZ9jzeV46EFv, 5, map[kbucket:5]
2   QmegNYmwHUQFc3v3eemsYUVf3WiSg4RcMrh3hovA5LncJ2, 5, map[kbucket:5]
3   QmU5ojUNUNhebeBB9yjv9NgEmfQBvjwbnfV4TJK45u9fkB, 5, map[kbucket:5]
4   QmNzVDdvnvaASTreEBisLFAjW3FPardRE8sw4GpcJquTGn, 5, map[kbucket:5]
5   QmY45XicRrvD1SoJ78xU6cBFGTm4TfKzhXVZBhxkqJC2gH, 5, map[kbucket:5]
6   QmUHsVLrBrDnXhuzJCSaxJiA4QXB9wqUKpTFcvWB7ruNdK, 5, map[kbucket:5]
7   QmdpGkbqDYRPCcwLYnEm8oYGz2G9aUZn9WwPjqvqw3XUAc, 5, map[kbucket:5]
8   QmQeREXWSZYgmSHyLCgRqEeHwJj8Z4eq2n1TuA1ra8i14m, 5, map[kbucket:5]
9   QmXemmxtH47JUEnvXofobGKE9JEp5WnfAK9ybvfGoXE2ki, 5, map[kbucket:5]
10  QmdLodSkopRTpuQREZBczbkmbYHRM8meRTsBLDebs93dse, 5, map[kbucket:5]
11  QmUnwhbFhkvgTd8XvaY2UGThBvHST8nPDRXURPZjtgiPur, 5, map[kbucket:5]
12  QmYzpoEDCZWAxFbzhjtu99qtFwHeU5oGUozTFE5quNd2Wi, 5, map[kbucket:5]
13  16Uiu2HAmGsma99vu8SaheLdCEvMAH2VGbiQ1UH75ctjEVyz89ck6, 5, map[kbucket:5]
14  QmdHZPpcxGxqaWFYr9vs3upKuzBi3YjqyF7GZqsbTEg9VB, 5, map[kbucket:5]
15  QmTE17QqamTkLWPz1hin1tAveadfHFzGgfqjte6JaNx3BU, 7, map[kbucket:5 relay-hop:2]
16  QmVGX47BzePPqEzpkTwfUJogPZxHcifpSXsGdgyHjtk5t7, 0, map[]
17  QmTZ4tp9kE6RPvGWaRwn9k45yoGgox7X8ygzA5cYV1uLyi, 5, map[kbucket:5]
18  QmSpssfWNjmLJyNoeuN2QjBkih2rd9Z3dWxThu2ctMK5Ri, 5, map[kbucket:5]
19  QmWTPu7cq5dwTQTLd6FPxHbeVfin6F4HaVpXfQPh1CjiZk, 5, map[kbucket:5]
20  QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ, 5, map[kbucket:5]
21  QmNfqdULX8SCbQmnQbuaAsyy4d3xHn7cvshNi6htxjkWgi, 5, map[kbucket:5]
22  QmPRQD8DSZmVXHn9VUi2wwZHuiT4Lp8jSn6FQfUL29FLvy, 5, map[kbucket:5]
23  QmRFS7Kfo6Hn16faNFt8RgEqE2khrSBH6WdhZvzLWTWiYv, 5, map[kbucket:5]
24  Qmc353gHY5Wx5iHKHPYj3QDqHP4hVA1MpoSsT6hwSyVx3r, 5, map[kbucket:5]
25  QmUKnnGqUEkoZAViXSHJfH3QufqDtVbTPN3zho5vrLEWFW, 5, map[kbucket:5]
26  Qma1KdoY2pch24e3frhdFmcU1oJQgnFtYLTGHcjuYbYbgP, 5, map[kbucket:5]
27  QmeMKDA6HbDD8Bwb4WoAQ7s9oKZTBpy55YFKG1RSHnBz6a, 5, map[kbucket:5]
28  QmYk9yh9CUdZgFCprWbMoEibetq8eUWNLyZCFZxmAyLnmF, 5, map[kbucket:5]
29  QmeDEixdEaEusbXRqaxqVp9nmhQ3zd4zLauP3YmEzGCLA1, 5, map[kbucket:5]
30  QmRZMaxtyk2NfvTG8gR2K2RbZBVM3aduah3W3ha9g19rv2, 5, map[kbucket:5]
```

As you can see, this list is much larger. You can repeat these commands, watching the list of peers grow each time as your node joins the network. When you run `qri connect` _both_ IPFS and Qri are running at the same time as _libp2p protocols_, sharing peer connections for joint purposes. Peers that support both IPFS and Qri are automatically detected.

Let's check in on our webapp. The easiest way is check on progress is to leave your browser window open, but if we have IPFS installed, we can follow along with ipfs commands. Our webapp is going to be tranferred via _bitswap_, so let's have a look at what bitswap is doing with this command:

```
ipfs --api /ip4/127.0.0.1/tcp/5001 bitswap stat
```


Normally, `ipfs bitswap stat` is a command you'd need to be running `ipfs daemon` to see. In this case `qri connect` is running an IPFS node already, and we made a point of telling IPFS where to look with the `--api` flag. when running `bitswap stat`, you'll generally see one of two results: 

The point where we're still asking the network for the webapp:
```
bitswap status
	provides buffer: 0 / 256
	blocks received: 0
	blocks sent: 0
	data received: 0
	data sent: 0
	dup blocks received: 0
	dup data received: 0
	wantlist [1 keys]
		QmPXeMgRHLyvnmzex9bdyWgwFd6KwS5dfk62MAoe2QAKZu
	partners [17]
```

Or the point after we've fetched the webapp:
```
bitswap status
	provides buffer: 0 / 256
	blocks received: 7
	blocks sent: 0
	data received: 1214257
	data sent: 0
	dup blocks received: 0
	dup data received: 0
	wantlist [0 keys]
	partners [34]
```

You can repeat the `ipfs --api /ip4/127.0.0.1/tcp/5001 bitswap stat` command as much as you like, once the `wantlist` drops back to `[0 keys]`, we're in business. You should be able to reload `http://localhost:2505` and see the webapp, which has been sent to your computer of the distributed web.

This hits on a central theme in the relationship between Qri & IPFS: When we add dataset to Qri, each version defaults to being stored as carefully arranged files on IPFS. Qri datasets live in your IPFS _repository_, which means you can examine data Qri's adding with IPFS tools. This means you can give datasets to peers that _don't have Qri at all_. As an example try visiting this URL:

`https://app.qri.io/b5/world_bank_population`

Here you'll see a "read only" version of our webapp, this time hosted on the web. If you look right under the title of the dataset, you'll see a familiar-looking IPFS hash. Let's try something, copy-paste that `/ipfs/Qm...` value and add it to the end of `https://ipfs.io` in a web browser, like this:

`https://ipfs.io/ipfs/QmXSAfNYeJ2mugR1yJvAZo7YxjVKeJ2ks7PNP6R324wYfu`

This may seem odd, but you're looking at the same data. This dataset includes a _viz script_, which is HTML that Qri knows how to render, When the author of this dataset included a viz script, Qri both kept the template, and wrote a file into the dataset that IPFS recognizes and knows how to display.

It gets better. _All_ components of a Qri dataset are stored inside this IPFS hash. We can look at raw commit data:
`https://ipfs.io/ipfs/QmXSAfNYeJ2mugR1yJvAZo7YxjVKeJ2ks7PNP6R324wYfu/commit.json`

We can get the code that created this dataset:
`https://ipfs.io/ipfs/QmXSAfNYeJ2mugR1yJvAZo7YxjVKeJ2ks7PNP6R324wYfu/transform_script`

And even get the raw data:
`https://ipfs.io/ipfs/QmXSAfNYeJ2mugR1yJvAZo7YxjVKeJ2ks7PNP6R324wYfu/body.csv`

All of this is versioned, auditable, tweetable, self-contained, and consistent. _All_ Qri datasets are shaped this way, which means _any_ time you come across a qri dataset, you can expect to find these dataset components in the same place. This consistent dataset layout is the secret sauce that makes qri _interoperable_, so much so that Qri's default storage format is built on interoperation between IPFS & Qri. 

Cool. Let's see that in action by adding that dataset to our machine. First, let's list the datasets we have on Qri, just to see that we currently don't have `b5/world_bank_population`, let's list our datasets:

```
qri list
```

Not there. lovely. Again, press `q` at any time to get back to your terminal. Let's add `b5/world_bank_population` to our collection:

```
qri add b5/world_bank_population
```

We can run qri list to see the change, And while we're here, let's copy the hash shown in the list for some inspection in IPFS:
```
qri list
```

When we ran `qri add`, qri pinned this dataset version to our IPFS node. We can use `ipfs pin ls` to see this:
```
ipfs --api /ip4/127.0.0.1/tcp/5001 pin ls [paste hash here]
```

When we see `rescursive` at the end, this means we've pinned the dataset to our IPFS node. Running `qri add` both gets me the data, and let's me share it with others. If, say, someone from your office tries to get this dataset while you're running `qri connect`, your machine can serve it to them.

Now that we've added something locally, let's head back to the webapp, and we can now see some new options that weren't presented on the web, like the actual body data!


### Grabbing Single Versions

Finally, we should point out that what you're looking at is a single version of a dataset, not it's entire history of versions. This is a departure from traditional version control systems that default to loading entire histories.

Qri's smallest element that works in isolation is a _dataset_. Most other version control systems have a _respository_ as their smallest element. By focusing on a _dataset_ in isolation we get some massive advantages for versioning data:

* Individual datasets are faster to move around
* Any dataset can be compared to _any other dataset_, not just datasets that share a history.
* For very large datasets, it's ok to keep a single version locally, backing up or delete prior versions without breaking functionality

While there are many points where we want entire histories, when we just want to see a visualization, it doesn't make sense to load the 

Because Qri is solely focused on datasets, many tradeoffs that wouldn't make sense in traditional version control systems drive big advantages in this context.