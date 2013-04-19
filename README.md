python-remotestorage
====================

An implementation of remotestorage for Python, using a git backend.

Use at your own risk. This is 

The remotestorage implementation takes the form of two WSGI daemons, one for storage and one for authentication. The most convenient way (IMHO) to run these, is to have them supervised by daemontools, runit or supervisord. An http server such as nginx should forward the HTTP requests to the two daemons. 

auth.py 
=======

The auth daemon provides (oauth2 implicit grant tokens)[http://tools.ietf.org/html/rfc6749#section-4.2], and is able to verify these. 
Authorized clients and their permissions are currently stored in an .auth configuration file. 

A line in the .auth file consists of three entries:
    <md5> <module> <permissions>

* <md5> is an md5 hash generated by the auth module upon the first request. The user is instructed to add this to the configuration to authorize the service.
* <module> is a module to grant access to, or 'root'.
* <permissions> is a permissions string such as 'r' or 'rw'

If during a request the corresponding md5 hash is found in the file, access is granted and a token or code is provided to the client. 

rs.py
=====

This is the git remote storage daemon. It replies to any GET, PUT, DELETE and OPTION request according to the standard.

Storage is currently implemented by creating a git repository per module in the current directory. External git repositories are on the todo list. Any GET, PUT or DELETE operation results in a commit by the daemon.

