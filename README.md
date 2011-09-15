RedStore
========
Nicholas J. Humfrey <njh@aelius.com>

For the latest version of RedStore, please see:
<http://www.aelius.com/njh/redstore/>


What is RedStore ?
------------------
RedStore is a lightweight RDF triplestore written in C using the [Redland] library.

Features:

* SPARQL over HTTP support
* Supports the [SPARQL 1.1 RDF Dataset HTTP Protocol]
* Compatible with rdfproc command line tool for offline operations
* Only runtime dependancy is [Redland].
* Unit tests for most of the HTTP server code.


Usage
-----
    redstore [options] [<name>]
       -p <port>       Port number to run HTTP server on (default 8080)
       -b <address>    Bind to specific address (default all)
       -s <type>       Set the graph storage type (default hashes)
       -t <options>    Storage options
       -n              Create a new store / replace old (default no)
       -f <filename>   Input file to load at startup
       -F <format>     Format of the input file (default guess)
       -v              Enable verbose mode
       -q              Enable quiet mode
  
Start RedStore on port 8080, bound to localhost, using a new sqlite store:
    redstore -p 8080 -b localhost -n -s sqlite

Load a URI into the triplestore:
    curl --data uri=http://example.com/file.rdf http://localhost:8080/load

Add a file to the triplestore:
    curl -T foaf.rdf 'http://localhost:8080/data/foaf.rdf'

Add a file to the triplestore with full URI specified:
    curl -T foaf.rdf 'http://localhost:8080/data/?graph=http://example.com/foaf.rdf'

Add a file to the triplestore with type specified:
    curl -T foaf.ttl -H 'Content-Type: application/x-turtle' 'http://localhost:8080/data/foaf.rdf'
 
You can delete graphs with in the same manner, using the DELETE HTTP verb:
    curl -X DELETE 'http://localhost:8080/data/foaf.rdf'

Query using the [SPARQL Query Tool]:
    sparql-query http://localhost:8080/sparql 'SELECT * WHERE { ?s ?p ?o } LIMIT 10'


Requirements
------------

The minimum required versions of the [Redland] RDF Libraries are:

- [raptor2-2.0.1]
- [rasqal-0.9.25]
- [redland-1.0.14]


Installation
------------
RedStore uses a standard automake build process:

    ./configure
    make
    make install


Supported Storage Modules
-------------------------

You can use any of the [Redland Storage Modules] that supports contexts:

- [hashes]
- [mysql]
- [memory] (Default)
- [postgresql]
- [sqlite]
- [virtuoso]


License
-------

This program is free software: you can redistribute it and/or modify
it under the terms of the [GNU General Public License] as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.



[Redland]:                     http://librdf.org/
[Redland Storage Modules]:     http://librdf.org/docs/api/redland-storage-modules.html
[SPARQL Query Tool]:           http://github.com/tialaramex/sparql-query
[GNU General Public License]:  http://www.gnu.org/licenses/gpl.html

[SPARQL 1.1 RDF Dataset HTTP Protocol]:  http://www.w3.org/2009/sparql/docs/http-rdf-update/

[raptor2-2.0.1]:               http://download.librdf.org/source/raptor2-2.0.1.tar.gz
[rasqal-0.9.25]:               http://download.librdf.org/source/rasqal-0.9.25.tar.gz
[redland-1.0.14]:              http://download.librdf.org/source/redland-1.0.14.tar.gz

[hashes]:                      http://librdf.org/docs/api/redland-storage-module-hashes.html
[mysql]:                       http://librdf.org/docs/api/redland-storage-module-mysql.html
[memory]:                      http://librdf.org/docs/api/redland-storage-module-memory.html
[postgresql]:                  http://librdf.org/docs/api/redland-storage-module-postgresql.html
[sqlite]:                      http://librdf.org/docs/api/redland-storage-module-sqlite.html
[virtuoso]:                    http://librdf.org/docs/api/redland-storage-module-virtuoso.html
