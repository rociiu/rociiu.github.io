---
layout: post
title: "Import csv into postgres with copy command"
date: 2015-02-06 11:30:35 +0800
comments: true
categories: 
---

When deal with data related project, if often need to import data from csv or excel, you can write loop to import with active record model one by one or by transaction, you can also use database command to do the bulk import, in mongodb there's [mongoimport](http://docs.mongodb.org/v2.2/reference/mongoimport/), for postgresql you can use [copy](http://www.postgresql.org/docs/9.2/static/sql-copy.html).

Here I want to import crimes data ([http://data.police.uk](http://data.police.uk)) into postgresql, I wrote follow sql command to import:

    sql = "copy crimes (crime_id, month, reported_by, falls_within, lon, lat, location, lsoa_code, lsoa_name, crime_type, last_outcome_category, context) from '/Users/rociiu/sandbox/ukpolice/uk-crimes/xx.csv' DELIMITERS ',' CSV HEADER;"

    ActiveRecord::Base.connection.execute(sql) # run with active record connection

You can find more document with the link above, here I list column names following the table name to match the columns in csv, also specify the csv delimiters and tell the command that there's header in csv.
    
