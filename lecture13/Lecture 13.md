## Lecture 13

_**Back up files also need to be encrypted**_

* Remember that if you drop one table, you cannot restore it from a physical backup. It's all or nothing. 
  * _So physical and logical backup need not be seen as exclusive._

### Log shipping

> Remember that the log files (journal files) are files that are synchronously written whenever a transaction commits. They are used for bringing back database files to the image of memory after a crash. 

The idea is to periodically ship log files to a distant server that hosts a copy of the database and 

reapply them there. 

### Metrocluster

SAN systems manufacturers may offer metroclusters which may be disk-box-to-disk-box, physical replication that does only care about bytes, and not applications. It can be synchronous or asynchronous. 

> Basically, remember that if you want zero loss, replication must be synchronous  

### Arbitrage – _you can't have everything_ 

**Human errors can happen** .But if somebody does a big mistake on the primary database, the mistake may travel to the backup database even before you notice the problem. 

If problems are transferred too fast, you get **two corrupted databases** instead of one, and must restore from backups. 

> People hate to say that, but there is always a part of "bets" in solutions that are adopted. You can think of hybrid modes: transferring logs as soon as possible, but waiting a little before reapplying them. But how long should you wait? And it will impact the time to restart operations if you need to reapply logs in a hurry. 

### Decision Supporting System

**OLTP**(On Line Transaction Processing)

> Traditionally, systems that record orders, production, billing and so forth are called OLTP systems. They are characterized by short transactions that are executed a large number of times. 

**Don't maintain history records**

Consider **Long Term** and **Additional Data**, we'll have to deal with possibly far more data than operational databases. We need to **aggregate**.

**Data Warehouse**

> This repository should be distinct from operational databases not only because the schema might be different, but because the query patterns will be very different, with almost no repeated queries (or repeated only once a day, week or month) 

_**not "real time" data uploading schedule**_

![1](https://github.com/snowgy/DatabasePrincipleNotes/tree/master/lecture13/images/1.png)

**Break 3NF**

![3](https://github.com/snowgy/DatabasePrincipleNotes/tree/master/lecture13/images/3.png)

**Facts and Dimensions**

![4](https://github.com/snowgy/DatabasePrincipleNotes/tree/master/lecture13/images/4.png)

![2](https://github.com/snowgy/DatabasePrincipleNotes/tree/master/lecture13/images/2.png)

> Kimball's reasoning is the following: if I store a date column, that will be difficult to query. If I want to aggregate by date or month, I'll need to apply (complicated and different in all DBMS products) date functions that I'll never be able to index because most date functions are NOT determininistic. The day is likely to be my smallest time unit. What are 20 years, with one row per day? Under 7,000 rows? Very tiny today. Let's have one row per date, and decline each date under every possible form, and index every column. 

