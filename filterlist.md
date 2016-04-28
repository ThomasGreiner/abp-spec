# Filter list

```
filterlist = header *( CRLF *WSP line )
  header = "[Adblock" [ 1*WSP "Plus" 1*WSP 1*( 1*DIGIT / "." ) ] "]"
    ; /\[Adblock(?:\s*Plus\s*([\d\.]+)?)?\]/i
  line = comment / filter
    comment = "!" *WSP ( metadata / string)
      ; /^\s*!\s*(\w+)\s*:\s*(.*)/
      metadata = homepage  / title / expires / checksum / redirect / version
        homepage = "Homepage" *WSP ":" *WSP http-url
        title = "Title" *WSP ":" *WSP string
        expires = "Expires" *WSP ":" *WSP 1*DIGIT *WSP [ "h" ]
          ; /^(\d+)\s*(h)?/
        checksum = "Checksum" *WSP ":" *WSP md5
        redirect = "Redirect" *WSP ":" *WSP url
        version = "Version" *WSP ":" *WSP 1*DIGIT
    filter = blocking-filter / hiding-filter
      negatable-host = [ "~" ] host
      blocking-filter = [ "@@" ] ( domain-filter / regexp-filter ) [ "$" [ options ] ]
        domain-filter = *2"|" 1*charset
        regexp-filter = "/" regexp "/"
          ; /^(@@)?\/.*\/(?:\$~?[\w\-]+(?:=[^,\s]+)?(?:,~?[\w\-]+(?:=[^,\s]+)?)*)?$/
        options = option *( "," option )
          ; /\$(~?[\w\-]+(?:=[^,\s]+)?(?:,~?[\w\-]+(?:=[^,\s]+)?)*)$/
          option = associative-option / negatable-option / other-option
            associative-option = domain-option / sitekey-option
              domain-option = "domain=" negatable-host *( "|" negatable-host )
              sitekey-option = "sitekey=" negatable-sitekey *( "|" negatable-sitekey )
                negatable-sitekey = [ "~" ] sitekey
            negatable-option = [ "~" ] negatable
              negatable = "collapse" / "image" / "match-case" / "object" / "object-subrequest" / "other" / "script" / "stylesheet" / "subdocument" / "third-party"
            other-option = "document" / "elemhide" / "genericblock" / "generichide" / "ping" / "popup"
      hiding-filter = [ negatable-host *( "," negatable-host ) ] "#" [ "@" ] "#" css-selector
        ; /^([^\/\*\|\@"!]*?)#(\@)?(?:([\w\-]+|\*)((?:\([\w\-]+(?:[$^*]?=[^\(\)"]*)?\))*)|#([^{}]+))$/
        ; /^(.*?)(#\@?#?)(.*)$/

charset = %x21-7E
  ; CHAR = %x01-7F
  ; - CTL = %x00-1F / %x7F
  ; - CRLF = %x0D / %x0A
  ; - WSP = %x20 / %x09
sp-charset = charset / WSP
string = charset *sp-charset *1charset
url = <see http://tools.ietf.org/html/rfc3986#section-3>
http-url = <see http://tools.ietf.org/html/rfc3986#section-3>
  ; URL restricted to HTTP(S) protocol
host = <see http://tools.ietf.org/html/rfc3986#section-3.2.2>
  ; host part of a URI
md5 = <see https://www.ietf.org/rfc/rfc1321.txt>
  ; MD5 hash
regexp =
  ; regular expression (according to ECMAScript)
css-selector = <see http://www.w3.org/TR/CSS2/selector.html#selector-syntax and http://www.w3.org/TR/css3-selectors/#selector-syntax and http://www.w3.org/TR/css3-selectors/#lex>
  ; CSS selector
sitekey = %x30-39 / %x41-5A / %x61-7A / "+" / "/" / "="
  ; base64-encoded DER representation of RSA public key
```
