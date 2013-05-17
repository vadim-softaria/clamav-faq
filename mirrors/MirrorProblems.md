# Official Mirror Faq

## Here is a list of possible reasons why freshclam failed to fetch the latest updates:

* Invalid DNS reply. Falling back to HTTP mode or ERROR: Can't query current.cvd.clamav.net
	* There is a problem with your DNS server. Please check the entries in /etc/resolv.conf and verify that you can resolve the TXT record manually: $ host -t txt current.cvd.clamav.net If you can't, it means your network is broken. You'll be still able to download the updates, but you'll waste a lot of bandwidth checking for updates. Please note that some not RFC compliant DNS servers (namely the one shipped with the Alcatel (now Thomson) SpeedTouch 510 modem) can't resolve TXT record. If that's the case, please recompile ClamAV with the flag --enable-dns-fix .

* ERROR: Connection with ??? failed
	* Either your dns servers are not working or you are blocking port 53/tcp. You should manually check that you can resolve hostnames with: $ host database.clamav.net. If it doesn't work, check your dns settings in /etc/resolv.conf. If it works, check that you can receive dns answers longer than 512 bytes, e.g. check that your firewall is not blocking packets which originate from port 53/tcp. An easy way to find it out is: $ dig @ns1.clamav.net db.us.big.clamav.net

* ERROR: getfile: daily-*.cdiff not found on remote server
	* For some reason, the mirror didn't fetch the cdiff file. Freshclam can recover from this situation by trying the next mirror.

* WARNING: Incremental update failed, trying to download daily.cvd
	* For some reason, incremental update failed. Freshclam can recover from this situation by downloading the whole daily.cvd . No side effects.

* WARNING: Mirror xxx.xxx.xxx.xxx is not synchronized.
	* For some reason, this mirror has not fetched the latest updates yet. Freshclam can recover from this situation by trying the next mirror.

* Update failed. Your network may be down or none of the mirrors listed in freshclam.conf is working.
	* It's not your lucky day. Freshclam tried every possible way to download the updates but failed. Usually this means that all the mirrors in your local pool are down or not synchronized, or something really nasty happened. Please wait a few minutes and try again. Remember to pass the -v option to freshclam. It is also possible that you recently had a prolonged network outage and freshclam blacklisted all the mirrors: remove mirrors.dat from the DatabaseDirectory and try again. If the problem persists, check the "mirror status page":/mirrors.html and perhaps send the output of freshclam -v to mirror-admin at clamav dot net.

* too often connections with outdated version
	* Starting from ClamAV 0.9x, whenever your ClamAV engine becomes outdated and the difference between the functionality level required by the CVD and the functionality level supported by your ClamAV engine is more than 3, freshclam refuses to check for updates more often than 6 times per day. The reason for this is that there is no use to generate traffic on our mirrors if you cannot take advantage of all the signatures anyway. If you really care about catching as much malware as possible and you want to check for updates more often than 6 times per day, then you should also not run such an old version of ClamAV. This feature was introduced after our mirror sysadmins complained that really old versions of ClamAV were consuming more than 50% of their bandwidth to download some CVD updates that were almost useless to them (due to lack of support for mdb signatures).

* Your ClamAV installation is OUTDATED
	* This message does NOT indicate that you are unable to download the latest CVD update! You'll get this message whenever a new version of ClamAV is released. In order to detect all the latest viruses, it's not enough to keep your database up to date. You also need to run the latest version of the scanner. 

* WARNING: Current functionality level = 1, required = 2
	* The functionality level of the database determines which scanner engine version is required to use all of its signatures. If you don't upgrade immediately you will still be able to download the latest CVD updates but the engine won't be able to use ALL of them.

* Ignoring mirror <IP> (has connected too many times with an outdated version)
	* Certain users are experiencing database update issues due to the failed Authenticode database push. 
	* You can validate that you're having this particular issue by a number of ways: 
	* Check the hash of your daily.cvd. You are affected if the hash matches the following:
	* MD5: 89dedb45609e59b0244fb5202ab6fa56
	* SHA1: 9947ec90e60499ab7c3331670d5b26b4eaac76e4
	* Check your freshclam log file for repeated errors that look like:
	* Ignoring mirror xxx.xxx.xxx.xxx (has connected too many times with an outdated version)
	* Check the version number and the functional level of the daily.cvd by using sigtool:
	* sigtool --info daily.cvd will show a version number of 16681 and a functionality level of 73

	* If you are expereincing the problem, please do the following:  Stop the freshclam daemon if it's running, delete both mirrors.dat and daily.cvd, then restart the freshclam daemon. Freshclam will then download a new daily.cvd and will be up-to-date.
