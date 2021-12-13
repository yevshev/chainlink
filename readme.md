# Deploying Chainlink

A quick and dirty example of creating a local chainlink deployment composed 
of the following services:
- chainlink node
- postgresql db
- ethereum client
- external adapter for coingecko api
- external adapter for coinpaprika api

*Note: For local testing only, not to be used in production*

## Prerequisites
* git
* docker
* docker-compose

## Getting Started
The repo is based around a single yaml file, to make it easy to 
`docker-compose up`. 
```
git clone https://github.com/yevshev/chainlink.git
```
```
cd chainlink/
```
```
docker-compose up
```
## Logging into the UI
Point your browser to [localhost:6688](http://localhost:6688) to access the Web 
UI and log in with the example credentials in `/.api`:

`user@example.com` 

`password123`


## Bridges
Inside the Web UI you'll notice two bridges have already been created, one 
for each of our external adapters.  

In the `docker-compose.yml` file, under the `command` option, the shell script
includes the chainlink node CLI commands:
```
chainlink bridges create /chainlink/coingecko.json &&
chainlink bridges create /chainlink/coinpaprika.json
```
that created the bridges from the json definitions:
```json
{
    "name":"coinGecko",
    "url":"http://coingecko:8080"
}
```
```json
{
    "name":"coinPaprika",
    "url":"http://coinpaprika:8080"
}
```
## Jobs
An example chainlink job has also been created and it has already completed 
it's first run: A simple webook job that gets the price quote for ETH in USD, 
from both coingecko and coinpaprika, and calculates the average between the 
two values. 

The chainlink node CLI commands in the shell script under the `command` option
in the `docker-compose.yml` file:
```
chainlink jobs create /chainlink/quote_average.toml &&
chainlink jobs run 1
```

created and ran the job defined in the toml file
```toml
type = "webhook"
schemaVersion = 1
name = "ETH-quote-average"
observationSource = """

    coingecko_quote [type=bridge name="coingecko" requestData=<{"id": 0, "data": {"base": "ETH", "quote": "USD"}}>]
    parse_cg [type=jsonparse path="data,result"]

    coinpaprika_quote [type=bridge name="coinpaprika" requestData=<{"id": 0, "data": {"base": "ETH", "quote": "USD"}}>]
    parse_cp [type=jsonparse path="data,result"]

    coingecko_quote -> parse_cg -> quote_average
    coinpaprika_quote -> parse_cp -> quote_average

    quote_average [type=median]
"""
```

## Cleaning Up

```
docker-compose down
```