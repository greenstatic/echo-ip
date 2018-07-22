# Echo IP
A small Go web service to return the client's public IP.

Features:
- HTTP/HTTPS support
- Apache Common Log Format (CLF)
- No need for web server or reverse proxy
- GET & POST support

## Example Response
### Request
```http request
GET /
```
### Response
```json
{
    "success": true,
    "ip": "212.235.188.20",
    "datetime": "2018-07-22T21:24:11+02:00",
    "ipDetails": {
        "remoteIP": "212.235.188.20",
        "forwardedForIP": ""
    },
    "service": "echo-ip",
    "version": "1.0.0",
    "srcUrl": "https://github.com/greenstatic/echo-ip"
}
```

In case the HTTP request contains an `X-Forwarded-For` header, we will replace
the `ip` address response field with the specified value. This is useful in cases
where there is a proxy between the client and the server. However both IPs are
still explicitly defined in the `ipDetails` field.

## Server
```
A small go web service to return the client's public IP.'

Usage:
  echo-ip [flags]

Flags:
  -b, --bind ip          Bind to IP (IPv4/IPv6) (default 0.0.0.0)
  -c, --cert string      Server's HTTPS certificate
  -h, --help             help for echo-ip
  -p, --port uint16      Port to listen to
  -k, --privKey string   Server's certificate private key

```

By default the server will try to listen to port `80` (HTTP) however 
if both `--cert` and `--privKey` flags are supplied a HTTPS server 
on default port `443` will be started. The port can always be overridden
using the `--port` flag.

### Endpoints
- GET/POST /: IP details response (see example response)
- GET/POST /health: health information about the web service

Note: HTTP POST methods are supported in case the client wishes to bypass 
proxies that cache HTTP GET requests/responses but ignore HTTP POST.

## Download
You can download a complied version of the server on the releases page 
of this repository.

Note, this software is only for GNU/Linux.

## Future Features
- Let's Encrypt support to automatically refresh certificates and restart
the server
- Reverse IP lookup details in response
- IP address details in response (using something like GeoIP DB)
- Would be nice (necessary) to write some tests