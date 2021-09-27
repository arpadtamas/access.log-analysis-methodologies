# access.log analysis methodologies with linux commands


An example entry of an access.log file:
  
```
127.0.0.1 - - [21/Sep/2021:13:55:36 -0700] "GET /apache_pb.gif HTTP/1.0" 200 2326 "http://www.example.com/start.html" "Mozilla/4.08 [en] (Win98; I ;Nav)" 

```
Description of Fields:
- Field 1: User Address
- Field 2: RFC931
- Field 3: User Authentication
- Field 4: Date/Time
- Field 5: GMT Offset
- Field 6: Action
- Field 7: Return Code
- Field 8: Size
- Field 9: Referrer
- Field 10: Browser/Platform

With this methodology I'm using space as a delimiter, so the field numbers are differ from the cut command's f parameter's value.

# The cheatsheet:

Gather, sort and count the most frequent unique IP addresses:

```
cut -d " " -f 1 access.log | sort | uniq -c | sort -nr
```

Gather, sort and count the most frequent URLs:
```
cut -d " " -f 7 access.log | sort | uniq -c | sort -nr
```

Gather the unique User-Agent strings:
```
cut -d " " -f 12- access.log | uniq
```

Gather and sort the unique IP addresses between 2 timestamps:

```
sed -n '/21\/Sep\/2021:13:00/,/21\/Sep\/2021:15:25/p' access.log | cut -d " " -f 1 | sort | uniq
```

Count the RPS (request per second) by the occurence of each timestamp:
```
cut -d " " -f 4 access.log | sort | uniq -c | tr -d [
```

Calculate the amount of data traffic by timestamps:
```
awk '{sum[$4]+=$10} END {for (i in sum) print i,sum[i]}' access.log | sort 

```
