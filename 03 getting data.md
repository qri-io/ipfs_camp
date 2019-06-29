# 3 Analyzing peer counts the IPFS & libp2p network

IPFS is built on top of a _peer-to-peer network_. Unlike centralized services that all work through a single point of coordination & control, any peer is free to connect directly to anyone else on the network. This also means with the right tools it's possible for anyone to gather statistics on the network.

Gathering stats on the network involves some work. Generally you'd need to do the following:

1. write code that constructs a peer designed to _extract_ stats from the network
1. keep that peer continuously connected to the network to establish a time series of data
1. _transform_ that data into a format fit for analysis
1. choose some frequency to _load_ that data for analysis, possibly storing it in a database, or parsing a data file into an environment for analysis.

This is a classic example of an automated _extract, transform, load_ (ETL) pipeline. Before you can work with with p2p network data, you have to build such a pipeline. It takes time & effort, and getting it wrong can have dire consequences for your analysis.

What we really want is a continuously updating source of structured, reliable data. In an ideal world we'd skip the ETL stuff entirely, and spend time playing with data instead.

Today we're going to use qri to do just that.


### 3.1 GIVE DATA

Here's a link. let's click:
https://app.qri.io/b5/libp2p_node_count_hourly

This dataset is a version of the ETL pipeline we described earlier. Take a minute to read through the description. It's stored as JSON data, reports no errors, and has a tabular structure. Later we'll see this dataset includes a transform script that gives even more detail about it's creation.

Let's get that data on our computer. Since we're working with the command line, open a terminal add both datasets:

```
$ qri add b5/libp2p_node_count_hourly
$ qri add b5/libp2p_node_count_daily
```

Congrats, the latest version of the entire dataset is now on your computer. Simple, right?

### 3.2 GIVE DATA ELSEWHERE

Now that we have a version of the dataset locally, there are a bunch of things we can do with it. Let's jump straight to working with the data in something other than Qri. type the following and hit enter:

```
qri get body b5/libp2p_node_count --format csv > node_count.csv
```

This command fetches the node count data, converts it from it's stored json format to comma separated values (csv) format, and writes it to a file called `node_count.csv`. 

`qri get` grabs components of a dataset. In this case we're `get`ing the _body_ component, which is the "data" part of a dataset. There are other components (like _meta_ and _structure_ to name a few). Qri thinks of datasets like HTML documents, where the "body" is the component you usually care about most.

With these two commands we have a CSV file of recent p2p network stats ready for use. You can bring this into whatever environment you're most comfortable with & get to work. 

Let's answer a simple question with a common tool: how many peers are connected on average? We can get there with a spreadsheet application & one click.

Pick a spreadsheet program you have handy. Any of these will work:

* Microsoft Excel
* Numbers (Mac OS only)
* Google sheets

Each of these three programs will let you load the `node_count.csv` file, click a column header, and see the average in the bottom right hand corner. Congrats, you're now analyzing data in a tool of your choosing.

### 3.3 two way conversation 

With this data staring back at you, there are a number of questions you may start to ask as you click around, and that's the point.  At the time of writing `go-ipfs/0.4.21` is reporting something like 75-100 _thousand_ peers, while all other protocols are reporting far fewer. Wouldn't it be interesting to investigate that further?

This dataset has saved you the hassle of setting up an ETL pipleine. You can use the time you saved _not_ building this dataset to do analysis, gather addtional data, get a tasty beverage, whatever you like.

That time is saved because qri is a two way conversation that anyone can contribute to. You can use this data to help understand IPFS network behaviour, and if you publish those findings back. The community will thank you!