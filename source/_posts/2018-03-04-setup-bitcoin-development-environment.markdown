---
layout: post
title: "Setup bitcoin development environment"
date: 2018-03-04 18:16:09 +0800
comments: true
categories: 
---

Bitcoin or blockchain is crazy hot on internet today, and I'm curious what's the best way for a programmer to get start with it. After read some books, I found that you can setup a test bitcoin testnet and play with it, so in this post I will share how it works.

### Get a ubuntu server

First of all, you need to get a server or in your local environment. I'm running with latest ubuntu on a VPS server.

### Install docker-ce in ubuntu

I'm using docker-ce here. To install docker-ce in ubuntu, you can follow the steps:

#### remove the old docker if exists
```
$ sudo apt-get remove docker docker-engine docker.io
```

#### Update apt repos.
```
$ sudo apt-get update

```
#### Install dependencies packages

```
$ sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
$ sudo apt-get update
```



#### Install docker-ce
```
$ sudo apt-get install docker-ce
```


### Install bitcoin-testnet-box
#### Get the docker image.
```
docker pull freewil/bitcoin-testnet-box

```
#### Run bitcoin-testnet-box
```
docker run -t -i -p 19001:19001 -p 19011:19011 freewil/bitcoin-testnet-box
```
#### Then you will see something similar with:
```
tester@84074da951a2:~/bitcoin-testnet-box$
```
#### Check the info
```
make getinfo
```

### Generate blocks

Must generate 200 blocks to get balance
```
make generate BLOCKS=200
```

### Install ruby
I'm using ruby to call the rpc-json server, so ruby have to be installed in server first.
```
apt-get install ruby
```
### Run ruby script to bitcoin rpc-json

I got the code sample from bitcoin.it wiki, but replace the server with the username/password/post from the configuration in bitcoin-testnet-box. Save following code outside the docker instance as test.rb

``` ruby
require 'net/http'
require 'uri'
require 'json'
 
class BitcoinRPC
  def initialize(service_url)
    @uri = URI.parse(service_url)
  end
 
  def method_missing(name, *args)
    post_body = { 'method' => name, 'params' => args, 'id' => 'jsonrpc' }.to_json
    resp = JSON.parse( http_post_request(post_body) )
    raise JSONRPCError, resp['error'] if resp['error']
    resp['result']
  end
 
  def http_post_request(post_body)
    http    = Net::HTTP.new(@uri.host, @uri.port)
    request = Net::HTTP::Post.new(@uri.request_uri)
    request.basic_auth @uri.user, @uri.password
    request.content_type = 'application/json'
    request.body = post_body
    http.request(request).body
  end
 
  class JSONRPCError < RuntimeError; end
end
 
if $0 == __FILE__
  h = BitcoinRPC.new('http://admin1:123@127.0.0.1:19001')
  p h.getbalance
  p h.getinfo
  p h.getnewaddress
  p h.dumpprivkey( h.getnewaddress )
  # also see: https://en.bitcoin.it/wiki/Original_Bitcoin_client/API_Calls_list
end

```

Run the code:
```
ruby test.rb
```
You will see something like:
```
5539.9999616
{"version"=>130200, "protocolversion"=>70015, "walletversion"=>130000, "balance"=>5539.9999616, "blocks"=>211, "timeoffset"=>0, "connections"=>1, "proxy"=>"","difficulty"=>4.656542373906925e-10, "testnet"=>false, "keypoololdest"=>1520155323, "keypoolsize"=>100, "paytxfee"=>0.0, "relayfee"=>1.0e-05, "errors"=>""}
"mve5QiJZmVQTGqeYNwi3xf78vrVp7eArJL"
"cRtYox2ALixpCuhL7rdx7LzCKMKaA6gCbogVkb3YgsxBAdTYZwK6"
```



### References
* [https://github.com/freewil/bitcoin-testnet-box](https://github.com/freewil/bitcoin-testnet-box)
* [https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce-1](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce-1)
* [https://en.bitcoin.it/wiki/API_reference_(JSON-RPC)](https://en.bitcoin.it/wiki/API_reference_(JSON-RPC))