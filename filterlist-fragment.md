# Filter list fragment

```
filterlist-fragment = [ header ] *( CRLF *WSP line )
  line = comment / filter / instruction
    instruction = include
      include = "%include" *1WSP include-path "%"
        include-path = url / local-path
          local-path = [ source-name ":" ] path-in-source
            source-name = ( ALPHA ) *( ALPHA / DIGIT / "_" )

comment = <see comment in filterlist.md>
header = <see header in filterlist.md>
filter = <see filter in filterlist.md>
path-in-source = <see http://tools.ietf.org/html/rfc3986#section-3.3>
url = <see http://tools.ietf.org/html/rfc3986#section-3>
  ; URL restricted to HTTP(S) protocol
```
