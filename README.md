# Auction-POC

## Table of content
1. [Introduction](#introduction)
2. [Stack](#stack)
    - [Database](#database)
    - [HTTP Server](#httpserver)
    - [Websocket](#websocket)
    - [React front-end app](#front)
    - [Docker](#docker

## Introduction <a id="introduction"></a> 
<br>

This exercise is intended for new comers and its goal is to familiarize them with the current stack used at Drouot SI.
<br>

Goal : Create a simple auction app which allows a user to : 
* visualize auctions 
* see lot descriptions, estimations and highest bids
* bid on items 
<br>

When a user bids on a lot, all other users should see the new highest bid value. This should be enabled by a [websocket connection](Websocket).
<br>

Lots should have a :
* ID
* Estimate
* Initial offering
* Description
* Photo URL (You may use [cat](https://placekitten.com/) photos as placeholders)
<br>

## Stack <a id="stack"></a> 
<br>

### Database <a id="database"></a> 
<br>

This app should use a mysql database to persist the data. 
<br>

### HTTP Server <a id="httpserver"></a>
<br>

The back-end application is a golang http server with the following framework and libraries :
  * [Gin](https://github.com/gin-gonic/gin)
  * [Logrus](https://github.com/sirupsen/logrus)
  * [SQLX](https://github.com/jmoiron/sqlx) with [mysql driver](https://github.com/go-sql-driver/mysql)
  <br>

When the Server is started, it should populate the database with some placeholder data. <br>

### Websocket <a id="websocket"></a>
<br>

Ideally, a websocket upgrade endpoint should be added to the HTTP server. The [Gorilla Websocket](https://github.com/gorilla/websocket) package should be used to manage websockets.
<br>

Once the http request is upgraded, the websocket should listen for JSON serialized messages for :
  * Requesting next lot
  * Biding on current lot (lot ID, bid amount)
  * Getting current highest bid (on page init)
<br>


### React front-end app <a id="front"></a> 
<br>

This single page application should contain :
* a section for the current lot with its photo, description, estimation and current highest bid
* a button which allows the user to bid on the current lot (request sent through the websocket and on success, broadcast to all open websocket connections)
* a section with other lots in the auction (displayed as a mosaic or carousel). When an item/lot is clicked, a request for the new lot is sent through the websocket. The answer should update the current lot with the current highest bid.

### Docker <a id="docker"> <br>

Use [docker](https://www.docker.com/) to containerize the back-end application. [Docker-compose](https://docs.docker.com/compose/) should be used to allow access to the database.
