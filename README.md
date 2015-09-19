# tsws

TSWS, A Totally Simple Web Server in [Bash](https://www.gnu.org/software/bash/)
and [Socat](http://www.dest-unreach.org/socat/).

### Getting Started Example

    sudo apt-get install socat
    git clone https://github.com/dfletcher/tsws.git
    cd tsws
    ./tsws 127.0.0.1 8080 &
    chromium http://127.0.0.1:8080

### Installation Notes

Socat (`socat`) is required. Please install it using your platform-specific
package management tools.

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

TSWS_ROOT is optional, if empty no files will be served from disk and all content is in Bash function form.

### Content served through functions:

If the file from the url path does not exist, or TSWS_ROOT is not set, the
server looks for a function. Any characters other than [a-z|A-Z|0-9] will be
replaced with an underscore. The function name is prefixed with "www". The root
path in this scheme translates to "www_", so this is replaced by "www_index" in
order to have a nicer function name for the site index page. The function
should print text or binary content using `cat`, `echo`, `printf` or similar.

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

- All HTTP caching is ignored keep things simple.

- Parsing CGI variables off URLs is weak. TODO find a decent way to do it
  that doesn't baloon the example code.

- Iterative development is awkward. Altering functions while bash is
  running confuses it, and the server must be restarted to see changes.

### Upcoming v0.2 roadmap

- POST, file upload.

- Deprecate `nc`, remove docs (DONE).

- Improved URL parsing.

- Stuff request headers into map available in content functions.

- Detect envirornment, suggest course of action for missing deps.

- Improve code flow, _tsws_response is overly long and hard to read.

- It runs slow, investigate ways to improve performance.

- Bug fixes reported in github issue queue and via pull requests.
