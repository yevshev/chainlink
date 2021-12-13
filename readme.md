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


## Jobs
An example chainlink job has also been created and it has already completed 
it's first run: A simple webook job that gets the price quote for ETH in USD, 
from both coingecko and coinpaprika, and calculates the average between the 
two values. 

## Cleaning Up

```
docker-compose down
```