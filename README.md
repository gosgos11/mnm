### mnm is not mail[<img width="200" hspace="50" align="right" src="https://user-images.githubusercontent.com/458838/65545951-535f6980-decb-11e9-8f46-6122198097b0.png">](https://mnmnotmail.org)

The mnm project is building a legitimate replacement for email: 
a server (see below), 
a [client](https://github.com/networkimprov/mnm-hammer), and 
a [simple protocol](Protocol.md) between them.

Learn more at [mnmnotmail.org](https://mnmnotmail.org). 

[**Download the mnm client app**](https://mnmnotmail.org/#quick-start) 


### Server status

_13 April 2019_ -
A private preview is now live! Contact the author if you'd like to try it.

_19 August 2018_ -
After testing with mnm client, made a handful of fixes. Changed license to MPL.

_25 September 2017_ -
A [client application](https://github.com/networkimprov/mnm-hammer) is in development.

_3 August 2017_ -
A simulation of 1000 concurrent active clients 
delivers 1 million messages totaling 6.7GB in 46 minutes. 
It uses ~200MB RAM, <10MB disk, and minimal CPU time. 
Each client runs a 19-step cycle that does login, then post for two recipients (15x) 
or for a group of 100 (2x) every 1-30s, then logout and idle for 1-30s. 


### Quick start

1. Download & Build  
a) `go get github.com/networkimprov/mnm`  
b) `cd $GOPATH/src/github.com/networkimprov/mnm`

1. Enable TCP+TLS with self-signed certificate  
a) `openssl ecparam -genkey -name secp384r1 -out server.key`  
b) `openssl req -new -x509 -sha256 -key server.key -out server.crt -days 3650`  
c) `cp mnm.conf mnm.config` # edit to revise ntp.hosts and adjust listen.laddr with "host:port"

   Note: On a public Internet host, port 443 will see a steady trickle of probe requests 
   (often with malicious intent) which pollutes the mnm log. 
   Choose a port above 1024 to avoid this. 

1. Run server  
a) `./mnm` # default port 443 may require `sudo ./mnm`; logs to stdout & stderr  
b) _Ctrl-C_ to stop  
or  
a) `./mnm >> logfile 2>&1 &` # run in background, logs to end of logfile  
b) `kill -s INT <background_pid>` # send SIGINT signal, triggering graceful shutdown

1. Distribute the server address to users  
+&nbsp; For a self-signed certificate, the address is `=address:port`  
+&nbsp; For a proper certificate, the address is `+address:port`  
+&nbsp; Examples: `=192.168.1.2:3456` and `+example.com:443`


### Build & package

Assuming this repository has been obtained via `git clone`:

a) `cd mnm`  
b) `git stash` # if required  
c) `git checkout <your_branch>`  
d) Edit _kVersionDate_ in main.go  
e) `./pkg.sh` # make release downloads


### Testing

Continuous test sequence with simulated clients  
a) `./mnm 10` # may be 2-1000  
b) ctrl-C to stop


### What's here

- codestyle.txt: how to make Go source more clear
- qlib/: TMTP implementation
- test.json: qlib test data
- userdb.go: user & group records management
- userdb-test.go: userdb test procedure
- main.go: main(), network frontend
- mnm.conf: site-specific parameters; rename to mnm.config to enable TCP server
- mnm: the server executable
- After first run:  
  userdb/: user & group data  
  qstore/: queued messages awaiting delivery


### License

Copyright 2020 Liam Breck  
Published at https://github.com/networkimprov/mnm

This Source Code Form is subject to the terms of the Mozilla Public
License, v. 2.0. If a copy of the MPL was not distributed with this
file, You can obtain one at http://mozilla.org/MPL/2.0/

