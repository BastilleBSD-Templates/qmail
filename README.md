# qmail
Bastille Template for a simple one domain Qmail Mail Server

 STATUS: Testing

Last configuration steps

There a 2 files that needs to be changed before we can proceed. The first file is the one 
called smtpd_run in that file find the following line and replace xx.xx.xx.xx with your 
machines IP address.

	# options for tcpserver/sslserver
	IP=xx.xx.xx.xx
	PORT=25
	SSL=0
	SSL_CERT="$VQ/control/servercert.pem"
	SMTP_CDB="/etc/tcp/smtp.cdb"
	MAX=30
	# these require the "tcpserver limits"

Next we need to change the pop3 greeting which we do in this file pop3d_run please change 
this regardless if you are going to enable the pop3 service or not.
	
	#!/bin/sh
	PATH=/var/qmail/bin:/usr/local/bin:/usr/bin:/bin
	export PATH
	exec tcpserver -H -R -v -c100 \
	-x /etc/tcp/smtp.cdb \
	0 110 \
	qmail-popup domain.xyz \
	/usr/home/vpopmail/bin/vchkpw qmail-pop3d Maildir 2>&1

Set up some necessary mail aliases. Replace “domain.xyz” with the domain you would like these 
email to go to. Please don't skip this part as things may not work as intended if you do so.
	
	echo postmaster@domain.xyz > /var/qmail/alias/.qmail-root
	echo postmaster@domain.xyz > /var/qmail/alias/.qmail-postmaster
	echo postmaster@domain.xyz > /var/qmail/alias/.qmail-mailer-daemon

And finally run the script that puts everything in the right places.

	./qmail.sh



