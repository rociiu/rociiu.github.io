---
layout: post
title: "data visulazation - import data"
date: 2014-10-25 19:14:05 +0800
comments: true
categories: 
---

I have been worked on some data visualization projects in last few years for different kind of clients, used some different set of library/framework like backbonejs, raphaeljs, d3js.
Start from this post, i'll try to write down process on how we can do data visualization with modern technologies.

The data source will be [http://data.police.uk](http://data.police.uk) , which include different crimes records in UK.

## Install mongodb

Mongodb has many features like document, schemaless, query, etc, the reason i choose it is it's very easy to load the data into it using mongoimport.

    brew install mongodb

## Start mongodb

    mongod

## Create database

    use ukcrimes

## Download data from [http://data.police.uk/data/](http://data.police.uk/data/)

## Import data into database

    mongoimport --db ukpolice --collection crimes --type csv --headerline --file file_path

## Check imported data

    mongo
    use ukcrimes
    db.crimes.find()  

You'll see data like:

    { "_id" : ObjectId("544a7172d0e2b18dc73d060c"), "Crime ID" : "ca8de697ca1a5b0b1f76a3c0f3f652bf2e9e20a31fbf5af40df5de7fa55f63a4", "Month" : "2014-07", "Reported by" : "Avon and Somerset Constabulary", "Falls within" : "Avon and Somerset Constabulary", "Longitude" : "", "Latitude" : "", "Location" : "No location", "LSOA code" : "", "LSOA name" : "", "Outcome type" : "Offender given penalty notice" }
    { "_id" : ObjectId("544a7172d0e2b18dc73d060d"), "Crime ID" : "c50470f2a1af2a5fffaa3dd69218777200c1a9289b87dd36849325ed2585472e", "Month" : "2014-07", "Reported by" : "Avon and Somerset Constabulary", "Falls within" : "Avon and Somerset Constabulary", "Longitude" : "", "Latitude" : "", "Location" : "No location", "LSOA code" : "", "LSOA name" : "", "Outcome type" : "Offender given a drugs possession warning" }
    { "_id" : ObjectId("544a7172d0e2b18dc73d060e"), "Crime ID" : "b7458f39eb58018af53c85fd481aa2028e247baa30ceb1f095437359e78b63f6", "Month" : "2014-07", "Reported by" : "Avon and Somerset Constabulary", "Falls within" : "Avon and Somerset Constabulary", "Longitude" : "", "Latitude" : "", "Location" : "No location", "LSOA code" : "", "LSOA name" : "", "Outcome type" : "Suspect charged" }
    { "_id" : ObjectId("544a7172d0e2b18dc73d060f"), "Crime ID" : "3093f07675cd62ce39cc8eeb80261edb15d00cbee2bf2ece8d295ac6a0efab4e", "Month" : "2014-07", "Reported by" : "Avon and Somerset Constabulary", "Falls within" : "Avon and Somerset Constabulary", "Longitude" : "", "Latitude" : "", "Location" : "No location", "LSOA code" : "", "LSOA name" : "", "Outcome type" : "Suspect charged" }

## To understand the data columns, you can check [http://data.police.uk/about/](http://data.police.uk/about/)

## Query crimes stats

    db.crimes.group({key: { 'Crime type': 1}, reduce: function(curr, result){ result.total += 1}, initial: { total: 0 } })

result:

    [
	{
		"Crime type" : "Anti-social behaviour",
		"total" : 5593
	},
	{
		"Crime type" : "Burglary",
		"total" : 888
	},
	{
		"Crime type" : "Criminal damage and arson",
		"total" : 1215
	},
	{
		"Crime type" : "Other theft",
		"total" : 1343
	},
	{
		"Crime type" : "Violence and sexual offences",
		"total" : 2123
	},
	{
		"Crime type" : "Drugs",
		"total" : 292
	},
	{
		"Crime type" : "Public order",
		"total" : 577
	},
	{
		"Crime type" : "Shoplifting",
		"total" : 1019
	},
	{
		"Crime type" : "Vehicle crime",
		"total" : 689
	},
	{
		"Crime type" : "Possession of weapons",
		"total" : 22
	},
	{
		"Crime type" : "Bicycle theft",
		"total" : 271
	},
	{
		"Crime type" : "Other crime",
		"total" : 88
	},
	{
		"Crime type" : "Theft from the person",
		"total" : 172
	},
	{
		"Crime type" : "Robbery",
		"total" : 91
	}
    ]

