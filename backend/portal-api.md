# Viblast Portal REST API
#### (draft)

Statistics collected and aggregated by Viblast Tracker and
Viblast Aggregator is accessible in different ways.

Viblast Portal can be installed as part of the Viblast backend infrastructure
to provide REST API for collected data.


## Clients and Streams Descriptions

Viblast Portal supports grouping of streams by client. All streams that are
added to the system belong to given client.

### Get or Insert Client

To add a client to the system use *Get or Insert Client* call. You have
to use `POST` request to the following URL:

	POST /portal/tracker-api/clients/

The body of the request should contain the name of the client, e.g. "ACME TV",
and a client UUID, e.g. "11111111". Client UUID is used as reference key for
integration with external to Viblast Portal systems.

	{
		"name": "ACME TV",
		"uuid": "11111111"
	}

The response to this request contains the client Id into Viblast Portal:

	{
		"id": 1
	}

If the client already exists this request returns Viblast Portal
client Id.


### Get or Insert Stream

Streams are added to Viblast Portal by client using *Get or Insert Stream*
call. To add a stream you have to use `POST` request to:

	POST /portal/tracker-api/streams/

The body of the request should contain client Id, received from
*Get or Insert Client* call and CDN stream URL.

	{
		"client": 1,
		"cdn_stream": "http://cdn2.viblast.com/streams/big_buck_bunny/adaptive/975k/stream.m3u8"
	}

In response to this request Viblast Portal returns stream Id:

	{
		"id": 1
	}

If the stream already exists this request returns Viblast Portal
stream Id


### Get or Insert Stream Quality

When the stream supports addaptive bitrate stream qualities can be
added using *Get or Insert Stream Quality* call. To add a stream quality
you have to use `POST` request to:

	POST /portal/tracker-api/stream-qualities/

In the body of the rquest you have to specify stream Id and the quility
bitrate:

	{
		"stream": 1, 
		"bitrate": 975
	}

The qualit bitrate is expected as *integer* parameter.

In response to this call Viblast Portal returns stream quality Id.

	{
		"id": 1
	}


## Time Series Statistics

To get time series statistics for given stream we need to specify following
parameters:

* **time interval** - we need to specify start and end of the time interval we
  are interested in;
* **metric name** - Viblast Aggregator precomputes reports for different metrics
  and we have to choose which metric we are interested in;
* **client, stream and stream quality** - we need to specify which client or stream
  or stream quality we are interested in;

Reports are precomputed by Viblast Aggregator with minute time resolution.

### Time Interval Specification
To specify time interval for the time series data we are interested in,
we need to specify start date time `<from>` and end date time `<until>`.
The accepted date time format is unix timestamp. For example:

* `<from>`: `1431028976` - corresponds to `2015-05-07T23:02:56`
* `<until>`: `1431633776` - corresponds to `2015-05-14T23:02:56`

### Metric Specification

We need to specify the metric for time series data we want to receive.
Viblast Aggregator precomputes reports for various metrics. Example for
metrics currently available follow. More metrics could be added on client
request.

#### Concurrent Number of Viewers
* `full.count` - total number of viewer
* `webrtc.count` - total number of viewers that can be part of the PDN
  (that are users with WebRTC enabled browsers)

#### Concurrent Number of Viewers per Browser
* `browser.chrome.full.count` - number of viewers using Google Chrome browser
* `browser.firefox.full.count` - number of viewers using Mozilla Firefox browser
* `browser.ie.full.count` - number of viewers using Microsoft Internet Explorer browser
* `browser.safari.full.count` - number of viewers using Apple Safari browser
* `browser.opera.full.count` - number of viewers using Opera browser

#### Traffic Data
* `full.down_total` - total download traffic
* `full.down_cdn` - download traffic from the CDN
* `full.down_swarm` - download traffic from the PDN
* `full.up_swarm` - upload traffic to other peers

#### Traffic Data for PDN Viewers
* `webrtc.down_total` - total download traffic for WebRTC enabled viewers
* `webrtc.down_cdn` - download traffic from CDN for WebRTC enabled viewers
* `webrtc.down_swarm` - download traffic from PDN for WebRTC enabled viewers
* `webrtc.up_swarm` - upload traffic to PDF for WebRTC enabled viewers



### Client and Streams Specification

The client we are interested in is specified using `<client_id>`
received from *Get or Insert Client* request. We also can specify stream
and stream quality using `<stream_id>` obtained via *Get or Insert Stream*
and `<quality_id>` obtained via *Get or Insert Stream Quality*


## Time Series Requests

Time series reports aggregated for all streams and qualities for the given
client is available by `GET` request on the following URL:

	GET /portal/api/stats/time/<from>-<until>/<metric>/clients/<client_id>

Time series report for given stream is available by `GET` request
on the following URL:

	GET /portal/api/stats/time/<from>-<until>/<metric>/clients/<client_id>/streams/<stream_id>

Time series report for given stream quality is available through following
`GET` request:

	GET /portal/api/stats/time/<from>-<until>/<metric>/clients/<client_id>/streams/<stream_id>/qualities/<quality_id>


## Example Usage of the REST API

### Example: Number of Concurent Viewers for Client

If we want to get full number of viewers for given client with client Id
`11111111` for the time interval from `1431654240` to `1431654840` we
need to form following request:

	GET /portal/api/stats/time/1431654240-1431654840/full.count/clients/11111111
	
The body fo the response will contain JSON list of pair of values:

	[
		[535.0, 1431654300],
		[531.0, 1431654360],
		[533.0, 1431654420],
		[529.0, 1431654480],
		[527.0, 1431654540],
		[533.0, 1431654600],
		[534.0, 1431654660],
		[535.0, 1431654720],
		[535.0, 1431654780],
		[537.0, 1431654840],
		[null, 1431654900]
	]

Each pair contains the value and the timestamp - `[535.0, 1431654300]`. In
our example the value is the number of viewers `535.0`, and the timestamp
is `1431654300`. If there are no value for given timestamp (for example
if the timestamp is in the future), `null` value will be returned.

The returned data is with minute resolution.

### Example: Number of Concurent WebRTC Viewers for Client

To get number of concurent WebRTC viewers we need to form following
request:

	GET /portal/api/stats/time/1431654240-1431654840/webrtc.count/clients/11111111

The response is similar to the previous example:

	[
		[438.0, 1431654300],
		[435.0, 1431654360],
		[435.0, 1431654420],
		[432.0, 1431654480],
		[428.0, 1431654540],
		[436.0, 1431654600],
		[435.0, 1431654660],
		[436.0, 1431654720],
		[438.0, 1431654780],
		[437.0, 1431654840],
		[null, 1431654900]
	]


### Example: Download Traffic for Client
If we want to get download traffic from the CDN aggregated for all viewers
we have to form following request:

	GET /portal/api/stats/time/1431654240-1431654840/full.down_cdn/clients/11111111

The response will contain JSON list of pairs of values:

	[
		[2158622720.0, 1431654300],
		[2359073792.0, 1431654360],
		[2503929856.0, 1431654420],
		[2515304448.0, 1431654480],
		[2211820544.0, 1431654540],
		[2168904704.0, 1431654600],
		[2057329664.0, 1431654660],
		[2403556352.0, 1431654720],
		[2362773504.0, 1431654780],
		[2257200128.0, 1431654840],
		[null, 1431654900]
	]

Each pair `[2158622720.0, 1431654300]` contains traffic value
`2158622720.0` and a timestamp `1431654300`. Traffic value shows
the traffic generated by all peers for the timestamped minute in
**bytes**.


### Example: Download Traffic for Client from WebRTC Viewers

The request is similar to the previous example:

	GET /portal/api/stats/time/1431654240-1431654840/webrtc.down_cdn/clients/11111111

Response contains the data:

	[
		[1720442880.0, 1431654300],
		[1863022592.0, 1431654360],
		[1974493184.0, 1431654420],
		[2028833792.0, 1431654480],
		[1775206400.0, 1431654540],
		[1710138368.0, 1431654600],
		[1621176320.0, 1431654660],
		[1892342784.0, 1431654720],
		[1875288064.0, 1431654780],
		[1821089792.0, 1431654840],
		[null, 1431654900]
	]


### Example: Number of Concurent Viewers for Stream

The request for getting data for given stream is similar to previous
examples. The main difference is that you need to specify stream Id:

	GET /portal/api/stats/time/1431654240-1431654840/webrtc.count/clients/11111111/streams/1950124

The response will contain data for viewers of the specified stream
in minute resolution:

	[
		[29.0, 1431654300],
		[30.0, 1431654360],
		[30.0, 1431654420],
		[30.0, 1431654480],
		[30.0, 1431654540],
		[30.0, 1431654600],
		[30.0, 1431654660],
		[30.0, 1431654720],
		[30.0, 1431654780],
		[31.0, 1431654840],
		[null, 1431654900]
	]

### Example: Download Traffic for Stream generated by WebRTC viewers

The request for getting download traffic generated by viewer of given
stream is similar to the previous examples:

	GET /portal/api/stats/time/1431654240-1431654840/webrtc.down_cdn/clients/11111111/streams/1950124

The response will contain JSON list of timestamped data:

	[
		[184758272.0, 1431654300],
		[261952512.0, 1431654360],
		[324497408.0, 1431654420],
		[298819584.0, 1431654480],
		[138602496.0, 1431654540],
		[154801152.0, 1431654600],
		[163495936.0, 1431654660],
		[290296832.0, 1431654720],
		[288438272.0, 1431654780],
		[301142016.0, 1431654840],
		[null, 1431654900]
	]
