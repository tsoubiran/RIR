# IP Addresses Historical Database

This repository provides an historical database of allocations and assignments of Internet Protocol addresses made by Regional Internet Registries —RIR— to local Internet registry —LIR—. 
The database maps IP ranges to country from 2003 to 2020 thus enabling, for instance, country geolocalization of historical IP data.

## Details

RIR publish the state of per country allocation and assignment of Internet numbers on a daily basis. Those files are available from the RIR respective ftp servers :

* AfriNIC  : <ftp://ftp.afrinic.net/pub/stats/>
* APNIC    : <ftp://ftp.apnic.net/pub/stats/apnic/>
* ARIN     : <ftp://ftp.arin.net/pub/stats/arin/>
* LACNIC   : <ftp://ftp.lacnic.net/pub/stats/>
* RIPE NCC : <ftp://ftp.ripe.net/ripe/stats/>

In all, that's over 30,000 files which makes it impractical to work with over a long period of time. In addition, there are usually only a few daily allocation updates —if any updates at all—. This means that there is a lot of redundancy between files because each file is very similar to the one from the day before or the day after. 
Therefore, in order to save space and speed up queries, archived files were compiled into a single file recording only changes in the address space using the R IP package. 

The database records changes in :

* country assignment of an address range
* status of an address range —reserved, allocated, assigned—
* address range when it is either removed from or (re-)allocated to the pool entirely or by parts

So that, in the end, we only have about 460,000 lines instead of more than 949,555,000.

RIR archived records usually start at various points in time in 2003 except for AfriNIC that began operation in 2005 :

* AfriNIC  : 2005-02-18
* APNIC    : 2003-10-09
* ARIN     : 2003-11-20
* LACNIC   : 2004-01-01
* RIPE NCC : 2003-11-26

## Data layout

File layout is the same as the original files' with five additional fields (*)

|Name|Type|Description|
|----|----|-----------|
|ipr.oid|character|IP range OID (*)|
|registry|character|Registry name|
|cc|character|ISO 3166 2-letter country code|
|type|character|Type of Internet number resource represented in this record|
|start|character|IPv4 or IPv6 'first address' of the range|
|value|integer|count of hosts for this range, not necessarily a CIDR range|
|date|Date|Date on this allocation/assignment was made by the RIR in the format YYYYMMDD|
|status|character|One of allocated, assigned|
|dtstart|Date|The date the IP range first appeared (*)|
|dtend|Date|The date the IP range last appeared (*)|
|current|logical|whether the IP range was still allocated or assigned on the last recorded day (*)|
|ipr|character|Character representation of the IP range (*)|

Please refer to the RIR-Statistics-Exchange-Format.txt file for more information about RIR statistics files. 
Note that some ranges are not CIDR ranges. In that case, the IP are represented with the low and high ends of the range separated by a dash &ndash;eg 193.16.186.0-193.16.191.255.

Each line of each file of the database records an allocated IP range for a given period of time and the country code of the organization it was delegated to. For instance, the RIPE NCC ipv4 table starts with :

|ipr.oid|registry|cc|type|start|value|date|status|dtstart|dtend|current|ipr|
|---|---|---|---|---|---|---|---|---|---|---|---|
|RI2003-11-26-05994|ripencc|DK|ipv4|193.111.120.0|512|2002-04-22|assigned|2003-11-26|2003-11-28|FALSE|193.111.120.0/23|
|RI2003-11-26-06638|ripencc|NO|ipv4|193.201.144.0|128|2002-08-23|assigned|2003-11-26|2003-11-28|FALSE|193.201.144.0/25|
|RI2003-11-26-06447|ripencc|UK|ipv4|193.188.134.160|  8|1998-06-15|assigned|2003-11-26|2003-12-01|FALSE|193.188.134.160/29|
|RI2003-11-26-06925|ripencc|UK|ipv4|193.254.0.0| 16|1997-07-09|assigned|2003-11-26|2003-12-01|FALSE|193.254.0.0/28|
|RI2003-11-26-06926|ripencc|UK|ipv4|193.254.0.16| 16|1997-07-24|assigned|2003-11-26|2003-12-01|FALSE|193.254.0.16/28|

From the first line we can see that the 193.111.120.0/23 range was first delegated on 2002-04-22 and was assigned to Denmark from 2003-11-26 —which, in this case, is the date of the first RIPE file— to 2003-11-28. 

Files are located in the stats subdirectory of each RIR directory and are available in two formats :

* R RDS format
* plain text files with quoted values separated by a vertical bar | —U+007C—

In addition, the rir/stats subdir contains every other stats files combined into one.

## License

The database is licensed under the terms of the [Creative Commons Attribution-ShareAlike (BY-SA) ](https://creativecommons.org/licenses/by-sa/4.0/) license.


