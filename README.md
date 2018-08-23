stupid-proxy
============

A simple routing proxy in Go.  Accepts incoming connections on ports 80 and 443.

* Connections on port 80 are assumed to be HTTP.  A hostname is extracted from each using
the HTTP "Host" header.
* Connections on port 443 are assumed to be TLS.  A hostname is extracted from the 
server name indication in the ClientHello bytes.  Currently non-TLS SSL connections 
and TLS connections without SNIs are dropped messily.

Once a hostname has been extracted from the incoming connection, the proxy looks up
a set of backends on a consul server, which is assumed to be running on 127.0.0.1:8500.
The key for the set is `protocall/subdomain/ i.e. https/test.example.com for https://test.example.com

A backend is then selected at random from the list that was supplied by redis, and
the whole client connection is sent down to the appropriate port on that backend. The
proxy will keep proxying data back and forth until one of the endpoints closes the 
connection.


MIT licensed, in case you're crazy enough to want to use it for something :-)
