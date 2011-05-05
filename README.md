# node-mdb
 
node-mdb is a re-implementation of M/DB, the Open Source clone of SimpleDB

It has been completely re-written in Node.js Javascript and uses the free, Open Source GT.M database as the data repository.

Rob Tweed <rtweed@mgateway.com>  
05 May 2011, M/Gateway Developments Ltd [http://www.mgateway.com](http://www.mgateway.com)  

Twitter@ @rtweed

Google Group for discussions, support, advice etc: [http://groups.google.co.uk/group/mdb-community-forum](http://groups.google.co.uk/group/mdb-community-forum)

See [http://www.mgateway.com/mdb.html](http://www.mgateway.com/mdb.html) for background on M/DB.

## Important Note

This is an early release of *node-mdb* and does not currently support the entire range of SimpleDB APIs.  The APIs implemented so far are:

- CreateDomain
- DeleteDomain
- ListDomains
- DomainMetadata
- PutAttributes
- GetAttributes
- DeleteAttributes

Select can be used, but the only expression that is accepted at present is:

      Select * from [yourDomainName]

Future versions will include more of the SimpleDB APIs.

The current version uses HTTP, but could be adapted for use with HTTPS
	  
## License

Copyright (c) 2004-10 M/Gateway Developments Ltd,
Reigate, Surrey UK.
All rights reserved.

http://www.mgateway.com
Email: rtweed@mgateway.com

This program is free software: you can redistribute it and/or modify it under the terms of the GNU Affero General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License along with this program.  If not, see <http://www.gnu.org/licenses/>.

## Installing the GT.M Database

This Free Open Source version of node-mdb is designed for use with the GT.M database [http://fisglobal.com/Products/TechnologyPlatforms/GTM/index.htm](http://fisglobal.com/Products/TechnologyPlatforms/GTM/index.htm). 

You can download GT.M from [http://sourceforge.net/projects/fis-gtm/](http://sourceforge.net/projects/fis-gtm/).  Installation instructions are provided in the download kit.

However, the quickest and easiest way to get a GT.M system going is to use Mike Clayton's *M/DB installer* for Ubuntu Linux which will create you a fully-working environment within a few minutes.  Mike's installer actually installs the previous version of M/DB as well as GT.M, but the instructions later in this ReadMe document explain how to upgrade to the new Node.js-based version.

Node.js can reside on the same server as GT.M or on a different server.

The instructions below assume you'll be installing Node.js and GT.M on the same server.

You can apply Mike's installer to a Ubuntu Linux system running on your own hardware, or running as a virtual machine.  However, I find Amazon EC2 servers to be ideal for trying this kind of stuff out.  I've tested it with Ubuntu 10.10.

So, for example, to create an M/DB Appliance using Amazon EC2:

- Start up a Ubuntu Lucid (10.10) instance, eg use ami-508c7839 for a 32-bit server version, or ami-548c783d for a 64-bit server version.

**32-bit Ubuntu:**

- Log in to your Ubuntu system and start a terminal session. If you've started a Ubuntu 10.4 or 10.10 EC2 AMI, log in with the username *ubuntu*

        sudo apt-get update
        cd /tmp
        wget http://michaelgclayton.s3.amazonaws.com/mgwtools/mgwtools-1.11_i386.deb
        sudo dpkg -i mgwtools-1.11_i386.deb (Ignore the errors that will be reported)
        sudo apt-get -f install (and type y when asked)
        rm mgwtools-1.11_i386.deb
	 
	 
**64-bit Ubuntu:**

- Log in to your Ubuntu system and start a terminal session. If you've started a Ubuntu 10.4 or 10.10 EC2 AMI, log in with the username *ubuntu*

        sudo apt-get update
        cd /tmp
        wget http://michaelgclayton.s3.amazonaws.com/mgwtools/mgwtools-1.11_amd64.deb
        sudo dpkg -i mgwtools-1.11_amd64.deb (Ignore the errors that will be reported)
        sudo apt-get -f install (and type y when asked)
        rm mgwtools-1.11_amd64.deb

If you point a browser at the domain name/IP address assigned to the Ubuntu machine, you should now get the M/DB welcome screen.  Follow the instructions you'll see in the welcome screen to initialise M/DB.

You now have a fully-working and configured GT.M database, ready for use with node-mdb.  Now follow the instructions below to upgrade to the new Node.js-based version of M/DB.

## Installing Node.js

Install Node.js and NPM on your server.  See [http://nodejs.org](http://nodejs.org) for details.


## Installing the node-mdb components

- download the node-mdb repository, eg

      git clone git://github.com/robtweed/node-mdb.git

 The destination directory in which you'll find the files is determined by the path in which you ran the above command.

 In the repository's */lib* directory, you'll find the main M/DB Node.js file: *mdb.js*.  Copy this to the path */usr/local/gtm/ewd/* on your GT.M server (adjust this path appropriately if you've installed GT.M for use in another directory).

In the repository's */gtm* directory, you'll find the M/Wire interface files that are used by mdb.js to access the GT.M database (See [https://github.com/robtweed/node-mwire](https://github.com/robtweed/node-mwire) for more details on the *node-mwire* used by *node-mdb*)  

You need to copy the files to the following directories:

  - Copy the file *mwire* to */etc/xinetd.d/mwire*
  - Copy the file *zmwire* to */usr/local/gtm/zmwire* and change its permissions to executable (eg 755)
  - Copy the file *zmwire.m* to */usr/local/gtm/ewd/zmwire.m*
  
 Next, you need to set up the port that is used by M/Wire to access GT.M.  Edit */etc/services* and add the line:
 
       mwire  6330/tcp  # Service for M/Wire Protocol
 
Nearly there!  You just need to use NPM to install some other Node.js modules that are used by node-mdb.

## Installing the additional Node.js modules uses by node-mdb

Simply use NPM as follows:

        npm install node-mwire
		npm install redis-node
		npm install node-uuid
		
You're now ready to use *node-mdb*

## Running node-mdb

      cd /usr/local/gtm/ewd
	  node mdb.js
	  
Most SimpleDB clients will work with *node-mdb*.  The main requirement is that you must be able to change the URL that normally points to the Amazon SimpleDB server, and point it at your M/DB instance instead, eg:

      http://192.168.1.100:8081/
	  








 










## Background

M/DB is an Open Source clone of SimpleDB that uses the Open Source GT.M Mumps database as the storage engine.  M/DB behaves identically to SimpleDB, sharing its APIs.






    

