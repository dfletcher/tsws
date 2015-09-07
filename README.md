# tsws
TSWS, A Totally Simple Web Server in [Bash](https://www.gnu.org/software/bash/) and [Netcat](http://nc110.sourceforge.net/).

## Getting Started

    git clone https://github.com/dfletcher/tsws.git
    cd tsws
    ./tsws 127.0.0.1 8080 &
    chromium http://127.0.0.1:8080

## You can use TSWS as a library:

    TSWS_LIBRARY=1 . tsws
    declare www_index_Content_Type="text/html; charset=utf-8"
    function www_index {
      echo "<html><body>My index is better than the default.</body></html>"
    }
    tsws 127.0.0.1 8080

Have a look at [the source code](https://github.com/dfletcher/tsws/blob/master/tsws) for many more details and discussion, including theory of opreation and creating dynamic content.
