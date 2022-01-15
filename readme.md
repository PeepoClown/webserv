# Webserv

## Summary
This project is here to make you write your own HTTP server. You will
follow the real HTTP RFC and you will be able to test it with a real browser. HTTP is
one of the most used protocol on internet. Knowing its arcane will be useful, even if you
won’t be working on a website.

## Setup
To build and run project, use following commands in project dir:
```shell
make all
./webserv /path/to/config
```

![example](https://i.ibb.co/4mSgCHF/Screenshot-2022-06-13-at-03-08-41.png=x250)

## Constraints
* You must write a HTTP server in C++
* It must be conditionnal compliant with the rfc 7230 to 7235 (http 1.1) but you need
to implement only the following headers
  - Accept-Charsets
  - Accept-Language
  - Allow
  - Authorization
  - Content-Language
  - Content-Length
  - Content-Location
  - Content-Type
  - Date
  - Host
  - Last-Modified
  - Location
  - Referer
  - Retry-After
  - Server
  - Transfer-Encoding
  - User-Agent
  - WWW-Authenticate
* You can implement all the headers if you want to
* We will consider that nginx is HTTP 1.1 compliant and may be use to compare
headers and answer behaviors
* It must be non blocking and use only 1 select for all the IO between the client and
the server (listens includes).
* Select should check read and write at the same time.
* Your server should never block and client should be bounce properly if necessary
* You should never do a read operation or a write operation without going through
select
* Checking the value of errno is strictly forbidden after a read or a write operation
* A request to your server should never hang forever
* You server should have default error pages if none are provided
* Your program should not leak and should never crash, (even when out of memory
if all the initialisation is done)
* You can’t use fork for something else than CGI (like php or perl or ruby etc...)
* You can include and use everything in "iostream" "string" "vector" "list" "queue"
"stack" "map" "algorithm"
* Your program should have a config file in argument or use a default path
* In this config file we should be able to:
  - choose the port and host of each "server"
  - setup the server_names or not
  - The first server for a host:port will be the default for this host:port (meaning
  it will answer to all request that doesn’t belong to an other server)
  - setup default error pages
  - limit client body size
  - setup routes with one or multiple of the following rules/configuration (routes
  wont be using regexp):
    * define a list of accepted HTTP Methods for the route
    ∗ define a directory or a file from where the file should be search (for example
    if url /kapouet is rooted to /tmp/www, url /kapouet/pouic/toto/pouet is
    /tmp/www/pouic/toto/pouet)
    ∗ turn on or off directory listing
    ∗ default file to answer if the request is a directory
    ∗ execute CGI based on certain file extension (for example .php)
      - You wonder what a CGI is go? https://en.wikipedia.org/wiki/Common_Gateway_In· Because you wont call the cgi directly use the full path as PATH_INFO
      - Just remember that for chunked request, your server need to unchunked it and the CGI will expect EOF as end of the body.
      - Same things for the output of the CGI. if no content_length is returned
        from the CGI, EOF will mean the end of the returned data.
      - Your program should set the following Meta-Variables
        * AUTH_TYPE
        * CONTENT_LENGTH
        * CONTENT_TYPE
        * GATEWAY_INTERFACE
        * PATH_INFO
        * PATH_TRANSLATED
        * QUERY_STRING
        * REMOTE_ADDR
        * REMOTE_IDENT
        * REMOTE_USER
        * REQUEST_METHOD
        * REQUEST_URI
        * SCRIPT_NAME
        * SERVER_NAME
        * SERVER_PORT
        * SERVER_PROTOCOL
        * SERVER_SOFTWARE
       - Your program should call the cgi with the file requested as first argument
       - the cgi should be run in the correct directory for relativ path file access
       - your server should work with php-cgi
    * make the route able to accept uploaded files and configure where it should be saved
