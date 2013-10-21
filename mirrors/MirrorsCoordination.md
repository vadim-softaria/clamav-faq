# Official mirror howto

## Bandwidth limit

You are encouraged to put some bandwidth limit on your ClamAV mirror vhost.

Many mirror sysadmins running the [Apache HTTP Server](http//httpd.apache.org/) find the [Bandwidth Mod](http://bwmod.sourceforge.net/) useful for this purpose. 

Here is an example config that will do the following:

  * Limit .cvd downloads to 40KB/s
  * Limit .cdiff downloads to 400KB/s
  * Allow max 50 simultaneous connections
  * Minimum download speed 20KB/s

### Apache ###

<pre><IfModule mod_bw>
  BandWidthModule On
  ForceBandWidthModule On
  LargeFileLimit .cvd 1 40000
  LargeFileLimit .cdiff 1 400000
  MaxConnection all 50
  MinBandwidth all 20000
</IfModule>
</pre>

_Note_ You can also use mod_cband to limit the download-speed.
The Source is available at http://cband.linux.pl/download/ or http://sourceforge.net/projects/cband/
Just run
<pre>./configure
make
make install
</pre>

Add the module to your apache-config by manually adding it to _/etc/apache2/httpd.conf_

<pre>LoadModule cband_module  /usr/lib/apache2/modules/mod_cband.so
</pre>
Edit the config for the vhost and add

<pre><IfModule mod_cband.c>
  CBandSpeed 20kb/s 100 300
  CBandRemoteSpeed 20kb/s 100 300
</IfModule></pre>

This limits the download speed to 20kb/s, allows 100 requests/second and a maximum of 300 connections.

To improve mod_cband's performance add.

<pre>CBandScoreFlushPeriod 1
CBandRandomPulse On</pre>

to _/etc/httpd/conf/httpd.conf_

Done. Now restart apache.

If you would like a scoreboard, set something like.

<pre><IfModule mod_cband.c>
  CBandSpeed 20kb/s 100 300
  CBandRemoteSpeed 20kb/s 100 300
  <Location /cband-status>
    SetHandler cband-status
  </Location>
</IfModule></pre>

Create the directory for CBandScoreboard and make it writeable by the apache-user.

<pre>
  $ chown wwwrun.www /srv/www/scoreboard/
</pre>

The status page can be found on _http://your-domain/cband-status_.

With mod_cband you can also limit the download speed based on monthly traffic or the source-ip. For more information see http://codee.pl/cband_documentation.html

_Thanks to Florian Schaal_


### Lighttpd ###

<pre>$HTTP["url"] =~ "\.cvd$" {
  server.max-connections = 50
  connection.kbytes-per-second = 40
}
$HTTP["url"] =~ "\.cdiff$" {
  server.max-connections = 50
  connection.kbytes-per-second = 400
}
</pre>


### Nginx ###

<pre>
location / {
  root /home/clamavdb/public_html;

  location ~ \.cvd$ {
      limit_rate 40k;
  }

  location ~ \.cdiff$ {
      limit_rate 400k;
  }
}

</pre>

#### A customer example for Nginx ####

<pre>
limit_conn_zone $server_port zone=cvds:1m;
limit_conn_zone $server_port zone=cdiffs:1m;
limit_conn_log_level info;

server {
       server_name database.clamav.net ~^db\..*\.clamav\.net clamav.<hostname>.org;
       listen 80;

       access_log /var/log/nginx/clamav.log;

       location / {
               root /srv/www/clamav;
               index index.html;

               deny 80.66.20.180;
               deny 80.66.20.181;
               deny 194.0.92.9;
               deny 157.100.152.211;
               deny 190.152.195.59;
               deny 201.218.44.210;
               deny 204.108.158.210;
               deny 24.172.196.34;
               deny 75.101.5.9;
               deny 91.137.10.23;
               deny 83.246.65.151;
               deny 75.126.161.137;
               deny 204.108.158.210;
               deny 74.221.15.167;
               deny 12.204.130.4;
               deny 68.77.50.60;
               deny 74.63.151.6;
               deny 70.43.224.138;
               deny 58.185.8.93;
               deny 97.66.67.10;
               deny 173.12.136.201;
               deny 65.44.144.18;
               deny 203.97.179.125;
               deny 74.40.199.29;
               deny 75.77.27.234;
               deny 129.63.145.205;
               deny 74.211.27.58;
               deny 24.117.112.210;
               deny 190.187.29.114;
               deny 68.177.35.149;
               deny 67.134.121.34;
               deny 208.49.209.254;
               deny 68.110.78.146;
               deny 64.57.165.54;
               deny 74.94.92.21;
               deny 209.87.224.227;
               deny 69.54.28.39;
               deny 166.102.112.162;
               deny 64.52.68.176;
               deny 97.67.248.242;
               deny 195.246.8.48;
               deny 94.72.248.130;
               deny 69.199.198.74;
               deny 74.228.131.210;
               deny 157.100.229.29;
               deny 129.138.40.14;
               deny 24.96.199.186;
               deny 204.152.114.26;
               deny 200.144.56.120;
               deny 67.152.150.40;
               deny 75.150.239.73;
               deny 209.192.112.138;
               deny 99.62.192.121;
               deny 190.81.113.244;
               deny 173.9.201.101;
               deny 189.105.190.238;
               deny 189.54.13.107;
               deny 142.227.180.125;
               deny 76.85.204.183;
               deny 206.169.90.206;
               deny 24.12.176.109;
               deny 63.253.94.114;
               deny 82.94.100.154;
               deny 72.215.214.238;
               deny 98.67.150.187;
               deny 99.125.152.25;
               deny 75.175.196.182;
               deny 70.250.13.198;
               deny 82.93.128.84;
               deny 65.120.160.194;
               deny 190.152.116.155;
               deny 71.175.42.3;
               deny 69.64.58.140;
               deny 64.179.173.93;
               deny 81.192.114.50;
               deny 72.67.209.195;
               deny 187.114.126.248;
               deny 201.56.75.130;
               deny 128.46.20.30;
               deny 65.254.45.184;
               deny 196.28.59.204;
               deny 63.229.138.224;
               deny 187.35.140.69;
               deny 64.238.19.38;
               deny 213.136.114.254;
               deny 72.158.213.67;
               deny 70.182.240.67;
               deny 216.137.119.160;
               deny 69.120.239.192;
               deny 63.255.31.210;
               deny 93.159.243.2;
               deny 200.105.241.210;
               deny 208.106.81.109;
               deny 209.193.64.8;
               deny 75.154.173.119;
               deny 24.158.38.18;
               deny 216.58.226.250;
               deny 71.36.231.45;
               deny 87.66.105.205;
               deny 82.201.17.134;
               deny 82.106.157.138;
               deny 71.195.228.7;
               deny 71.53.177.146;
               deny 69.84.245.67;
               deny 69.245.156.9;
               deny 201.27.151.142;
               deny 108.16.98.3;
               deny 24.159.194.146;
               deny 66.68.64.109;
               deny 69.12.136.210;

               location ~ \.cvd$ {
                       limit_conn cvds 25;
                       limit_rate 40k;
               }

               location ~ \.cdiff$ {
                       limit_conn cdiffs 5;
                       limit_rate 400k;
               }

               # deny requests from versions <= 0.88 or devel
               if ( $http_user_agent ~* "^clam(av|win)\/(0\.[67]|devel-200[0-8]|devel-0\.[0-8]).*$" ) {
                       return 404;
               }

               # deny requests from versions <= 0.94 (not supported anymore)
               if ( $http_user_agent ~* "^clam(av|win)\/(0.9[01234]).*$" ) {
                       return 404;
               }
       }
}
</pre>

> With this, I have two zones, which I can configure independently and
> where I limit CVD downloads to 25 connections with each up to 40k.
> The cdiffs are limited to 5 connections at a time to up to 400k each.
> This results in about 2MB/s or 5TB a month. If we stay way under my 10TB
> limit, I will allow some more connections.
>
>The trick is to use a static value like $server_port as a counter
>instead of $remote_address.

# Reducing traffic #

## The centrally maintained blacklist ##

We provide a configuration file for Apache and Lighttpd which lists.

  * IP addresses that are reported by our mirrors as abusers
  * obsolete ClamAV installations

The names of the files are.

  * local_blacklist_apache - written following Apache mod_access syntax, it can be used as an .htaccess file
  * local_blacklist_lighttpd - meant to be included into lighttpd config.

Our script (clam-clientsync) by default excludes all files with a local_* prefix, so you won't see this file on your mirror.

You can write your own script to regularly download this file and use it on your webserver.

If you are running Apache and mod_access is enabled (most common setup), this can be as simple as this.

<pre>#!/bin/bash
. $HOME/etc/clam-clientsync.conf
export RSYNC_PASSWORD
rsync $RSYNC_USER@rsync.clamav.net::$MODULE/local_blacklist_apache $TO/.htaccess
</pre>

### Lighttpd ###

<pre>#!/bin/bash
. $HOME/etc/clam-clientsync.conf
export RSYNC_PASSWORD
rsync $RSYNC_USER@rsync.clamav.net::$MODULE/local_blacklist_lighttpd /path/local_blacklist_lighttpd</pre>

You will need to include local_blacklist_lighttpd into main.conf, like this.

<pre>$HTTP["host"] =~ "^(clamav\.yourhostname\.tld|.*\.clamav\.net)$" {                     
    include "/path/local_blacklist_lighttpd"
</pre>


## Additional tweaks ##


### Manually blackList old versions of ClamAV ###

We kindly ask our mirrors to support as many old versions of ClamAV as possible. However we understand that this can eat a lot of resources and not every mirror can afford it. Hereby we provide some config. examples for various web servers.


### Apache HTTP Server ###

<pre>SetEnvIfNoCase User-Agent "^clamav/0.6" bad_clamav 
SetEnvIfNoCase User-Agent "^clamav/devel-2008" bad_clamav
SetEnvIfNoCase User-Agent "^ClamWin/0.6" bad_clamav
</pre>

<pre><Location "/">
  Order allow,deny
  Allow from all
  Deny from env=bad_clamav
</Location>
</pre>


### Lighttpd ###

<pre>$HTTP["useragent"] =~ "^clam(av|Win)\/(0.[67]|.*devel).*$" {
  url.access-deny = ( "" )
}
</pre>


### Nginx ###

<pre>if ( $http_user_agent ~* "^clam(av|win)\/(0\.[67]|devel-200[0-8]|devel-0\.[0-8]).*$" ) {
  return 404;
}
</pre>


## Block outdated clients

Due to numerous connects of outdated clients (&gt; 300,000 / day), we add single IP temporarily to the firewall.

Requirements.

   * [Apache HTTP-Server](http://httpd.apache.org/)
   * [syslog-ng](http://www.balabit.com/)
   * [iptables](http://netfilter.org/projects/iptables/index.html)
   * Kernel with recent-support


## Configure Apache HTTP-Server ##


The Access Log of apache must send to syslog-ng:

<pre>$ mknod /var/log/apache/access.log p</pre>

### Apache HTTP Server (httpd.conf or vhost.conf) ###

The logformat and the accesslog must be defined like.

<pre>LogFormat "%h %v %l %u %t \"%r\" %&gt;s \"%{Referer}i\" \"%{User-Agent}i\"" syslog<br />CustomLog= =/var/log/apache2/access.log syslog
</pre>

As long as the log file runs only through the pipe, no entries are stored. The configuration used here evaluates merely the log file. To receive an Access Log as a file, you must extend either syslog-ng by a destination or the apache-config by a CustomLog.

## Configure syslog-ng ##

<pre>source s_apache_access { <br /> pipe("/var/log/apache2/access.log"); <br /> };<br /><br />destination d_clamav-403 { <br /> file("/proc/net/xt_recent/clamav-403"<br /> template("+${APACHE.SRC-IP}\n")); <br /> };<br /><br />filter f_clamav_403 { <br /> message('clamav.net') <br /> and message(' 403 '); <br /> };<br /><br />parser p_apache_src_ip { <br /> csv-parser(columns("APACHE.SRC-IP")<br /> delimiters(': ') <br /> flags(escape-none,greedy) <br /> template("${MSGHDR}") ); <br /> };<br /><br />log { <br /> source(s_apache_access);<br /> filter(f_clamav_403);<br /> parser(p_apache_src_ip);<br /> destination(d_clamav-403); <br /> };</pre>

## Iptables ##

<pre>iptables -A INPUT -p tcp --dport 80 -m recent --rcheck --name clamav-403 --seconds 3600 --hitcount 5 -j DROP</pre>

## how it works ##

Syslog-ng filters apache messages with the contents clamav.net and 403. As destination /proc/net/xt_recent/clamav-403 is defined. The template adds the IP to the firewall. With reach from "hitcount" the IP is blocked "seconds".

If you replace the _rcheck_ here with an _update_ statement, the block will last even longer. The _rcheck_ option means: we will block you for the next hour. While _update_ means: we don't want to see you for an hour, but if we see you again during this time, we'll block you again. It means that you actually need to be quiet for 60 minutes to be able to log in again.

By default xt_recent stores 100 IP addresses. You can change the limit with "modprobe ipt_recent ip_list_tot=10000" (here 10000). This is only possible before the first iptables rule is put on.

Use.
<pre>$ chmod 600 /sys/module/xt_recent/parameters/ip_list_tot
$ echo 10000 &gt; /sys/module/xt_recent/parameters/ip_list_tot
$ chmod 400 /sys/module/xt_recent/parameters/ip_list_tot
</pre>

to change ip_list_tot "on-the-fly"

If you use ipt_recent instead of xt_recent, be sure to modify the filenames and path.

Credits: [Valentijn Sessink](http://valentijn.sessink.nl/?p=322)
