# tsws

TSWS, A Totally Simple Web Server in [Bash](https://www.gnu.org/software/bash/)
and [Netcat](http://nc110.sourceforge.net/).

### Getting Started

    git clone https://github.com/dfletcher/tsws.git
    cd tsws
    ./tsws 127.0.0.1 8080 &
    chromium http://127.0.0.1:8080

### You can use TSWS as a library:

    TSWS_LIBRARY=1 . tsws
    declare www_index_Content_Type="text/html; charset=utf-8"
    function www_index {
      echo "<html><body>My index is better than the default.</body></html>"
    }
    tsws 127.0.0.1 8080

### You can serve files from your filesystem:

    TSWS_ROOT="/var/www/html" ./tsws

When serving files this way, the server first looks to see if a file exists
$TSWS_ROOT/$URL_PATH. If it does, and the file is actually inside TSWS_ROOT or
a subdirectory, it will be served up. If the file exists but is not underneath
TSWS_ROOT, a 500 error occurs.

When serving files from disk, `file -ib` is used to detect the Content-Type.

### Content served through functions:

If the file from the url path does not exist, the server looks for a function.
Any characters other than [a-z|A-Z|0-9] will be replaced with an underscore.
The function name is prefixed with "www". The root path in this scheme
translates to "www_", so this is replaced by "www_index" in order to have a
nicer function name for the site index page. The function should print text or
binary content using `cat`, `echo`, `printf` or similar.

When a function is used to serve content, the Content-Type is declared in a
variable with the same name as the function with a suffix "_Content_Type".

See the library example above for an example of a content serving function and related _Content_Type variable.

### Getting down and dirty:

Have a look at
[the source code](https://github.com/dfletcher/tsws/blob/master/tsws)
for many more details and discussion, including theory of operation and
creating dynamic content.

### Known issues and bugs

- !GAPING SECURITY HOLE WARNING!
  Do not trust the input. An attempt is made to scrub requests, it may not
  be 100% complete (this project is brand-spankin' new). The core engine
  looks for bash functions named similarly to the path. These are prefixed
  with "www_". Think carefully when writing functions named similarly,
  ensure that values coming from the network cannot be evaluated.

- This setup almost certainly has issues with concurrency. It is useful for
  a single local user or demonstration purposes, but please do not use it
  as a production web server connected to the internet.

- All HTTP caching is ignored keep things simple.

- Cygwin doesn't work so great. Particularly, some requests just fail
  inexplicably with nc never supplying a request or supplying a massively
  delayed one.

- It is easily confused, especially if multiple browsers or tabs are
  pointed to it at the same time. Requests and responses might get dropped
  (still not quite sure why on that) and the order can get out of whack,
  sending the wrong file for a request.

- Iterative development is awkward. Altering functions while bash is
  running confuses it, and the server must be restarted to see changes.

- Parsing CGI variables off URLs is weak. TODO find a decent way to do it
  that doesn't baloon the example code.

