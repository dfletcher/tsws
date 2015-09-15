# tsws

TSWS, A Totally Simple Web Server in [Bash](https://www.gnu.org/software/bash/)
and [Socat](http://www.dest-unreach.org/socat/) or alternatively [Netcat](http://nc110.sourceforge.net/).

## Getting Started Example

    sudo apt-get install socat
    git clone https://github.com/dfletcher/tsws.git
    cd tsws
    ./tsws 127.0.0.1 8080 &
    chromium http://127.0.0.1:8080

## Installation Notes

Socat (`socat`) is recommended over Netcat (`nc`) and Socat is required for
correct operation on the Cygwin platform. The script first checks for `socat`
and uses that if available. If not, it checks for `nc` and uses that as a
fallback solution.

### If you cannot install Socat

There are a lot of variations of the `nc` program around and it might not work.
If `nc` complains about a missing -k option, you could try calling the script in
a loop to emulate the operation:

    while : ; do ./tsws localhost 8080; done

This will not work well on Cygwin or other Windows based Bash distributions, the
delay of launching an executable will cause too much downtime, the browser will
often be trying to connect while this is looping and will get no response
because it's looping around to the front of this loop.

## You can use TSWS as a library:

    TSWS_LIBRARY=1 . tsws
    declare www_index_Content_Type="text/html; charset=utf-8"
    function www_index {
      echo "<html><body>My index is better than the default.</body></html>"
    }
    tsws 127.0.0.1 8080

Have a look at [the source code](https://github.com/dfletcher/tsws/blob/master/tsws) for many more details and discussion, including theory of opreation and creating dynamic content.

