## Quick reference

This image runs an easily configurable WebDAV and Samba server.

## Usage

```
docker build -t lemofire/naskit .

docker run -d --name naskit --restart always \
    -v /srv/dav:/var/lib/dav/data \
    -e AUTH_TYPE=Basic -e USERNAME=alice -e PASSWORD=secret \
    -e SHARE_NAME=naskit \
    -e SSL_CERT=selfsigned \
    -p 137:137/udp -p 138:138/udp -p 139:139 -p 445:445 -p 445:445/udp -p 4000:443 -d lemofire/naskit

```

### Environment variables

All environment variables are optional. You probably want to at least specify `USERNAME` and `PASSWORD` (or bind mount your own authentication file to `/user.passwd`) otherwise nobody will be able to access your WebDAV server!

* **`SERVER_NAMES`**: Comma-separated list of domains (eg, `example.com,www.example.com`). The first is set as the [ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername), and the rest (if any) are set as [ServerAlias](https://httpd.apache.org/docs/current/mod/core.html#serveralias). The default is `localhost`.
* **`LOCATION`**: The URL path for WebDAV (eg, if set to `/webdav` then clients should connect to `example.com/webdav`). The default is `/`.
* **`AUTH_TYPE`**: Apache authentication type to use. This can be `Basic` (best choice for HTTPS) or `Digest` (best choice for HTTP). The default is `Basic`.
* **`REALM`**: Sets [AuthName](https://httpd.apache.org/docs/current/mod/mod_authn_core.html#authname), an identifier that is displayed to clients when they connect. The default is `WebDAV`.
* **`USERNAME`**: Authenticate with this username (and the password below). This is ignored if you bind mount your own authentication file to `/user.passwd`.
* **`PASSWORD`**: Authenticate with this password (and the username above). This is ignored if you bind mount your own authentication file to `/user.passwd`.
* **`ANONYMOUS_METHODS`**: Comma-separated list of HTTP request methods (eg, `GET,POST,OPTIONS,PROPFIND`). Clients can use any method you specify here without authentication. Set to `ALL` to disable authentication. The default is to disallow any anonymous access.
* **`SSL_CERT`**: Set to `selfsigned` to generate a self-signed certificate and enable Apache's SSL module. If you specify `SERVER_NAMES`, the first domain is set as the Common Name.
* **`SHARE_NAME`**: Shared folder name shown in Samba.

### Credits

* [BytemarkHosting/docker-webdav](https://github.com/BytemarkHosting/docker-webdav)
* [P3TERX/docker-aria2-pro](https://github.com/P3TERX/docker-aria2-pro)
