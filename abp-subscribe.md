# abp:subscribe URIs

```
abp-subscribe = "abp:" *"/" "subscribe" *"/" "?" params
  ; /^abp:\/*subscribe\/*\?(.*)/i
  params = param *( "&" param )
    param = title / location / requiresTitle / requiresLocation
      title = "title=" uri-encoded
      location = "location=" url-encoded
        ; Note: This is the only required parameter
      title-main = "requiresTitle=" uri-encoded
      location-main = "requiresLocation=" url-encoded

url-encoded = scheme %3A%2F%2F uri-encoded
  scheme = "ftp" / "http" / "https"
decimal-digit = %x30-39
hex-digit = decimal-digit / %x41-46 / %x61-66

; according to ECMA-262
uri-encoded = 1*uri-character
  uri-character = uri-reserved / uri-unescaped / uri-escaped
    uri-reserved = ";" / "/" / "?" / ":" / "@" / "&" / "=" / "+" / "$" / ","
    uri-unescaped = uri-alpha / decimal-digit / uri-mark
    uri-escaped = "%" hex-digit hex-digit
    uri-alpha = %x41-5A / %x61-7A
    uri-mark = "-" / "_" / "." / "!" / "~" / "*" / "'" / "(" / ")"
```
