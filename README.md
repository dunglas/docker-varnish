# emgag/varnish

Legacy versions of varnish image. **These are unmaintained, unsupported versions. For currently supported images, see master branch.**

Shipped VMODs:
* [libvmod-digest](https://github.com/varnish/libvmod-digest): HMAC, hash and base64 functions
* [libvmod-dynamic](https://github.com/nigoroll/libvmod-dynamic): Dynamic backend director
* [varnish-modules](https://github.com/varnish/varnish-modules): Official varnish vmod collection (cookie,header,saintmode,softpurge,tcp,var,vsthrottle,xkey)
* [libvmod-geoip](https://github.com/varnish/libvmod-geoip): GeoIP lookup support **MaxMind no longer supports the legacy database format and the update mechanism in Debian contrib, so the images no longer contain a copy of the database and don't contain a built-in mechanism to update the GeoIP database**. 

## Tags and respective `Dockerfile` links

- [`4.1.11` (*4.1.11/Dockerfile*)](https://github.com/emgag/docker-varnish/blob/master/4.1/Dockerfile)
- [`5.2.1` (*5.2.1/Dockerfile*)](https://github.com/emgag/docker-varnish/blob/master/5.2/Dockerfile)
- [`5.1.3` (*5.1.3/Dockerfile*)](https://github.com/emgag/docker-varnish/blob/master/5.1/Dockerfile)
- [`5.0.0` (*5.0.0/Dockerfile*)](https://github.com/emgag/docker-varnish/blob/master/5.0/Dockerfile)

## Varnish

From [varnish-cache.org](https://varnish-cache.org/intro/index.html): _Varnish Cache is a web application accelerator also known as a caching HTTP reverse proxy. You install it in front of any server that speaks HTTP and configure it to cache the contents. Varnish Cache is really, really fast. It typically speeds up delivery with a factor of 300 - 1000x, depending on your architecture._

## How to use this image.

By default, varnish reads `/etc/varnish/default.vcl` on startup. Either copy your VCL file in your Dockerfile  
  
```
FROM emgag/varnish:4.1.11
COPY default.vcl /etc/varnish/default.vcl
```

or mount a volume containing the varnish configuration to `/etc/varnish`, e.g with a docker-compose file:

```
version: '3'
services:
  varnish:
    image: emgag/varnish:4.1.11
    volumes:
      - ./varnish:/etc/varnish
    ports:
      - "80:80"
```

Following environment variables can be used to customize the behaviour of the container:
* VARNISH_CONFIG (default: `/etc/varnish/default.vcl`): The VCL file read on startup.
* VARNISH_DAEMON_OPTS: Additional command line arguments for `varnishd`.
* VARNISH_LISTEN (default: `:80`): The TCP port to listen for incoming client connections. Make sure to also expose the new port if this value is modified.
* VARNISH_MANAGEMENT_LISTEN (default: `127.0.0.1:6082`): The TCP port to listen for management connections. See varnish documentation about [management interface authentication](https://varnish-cache.org/docs/trunk/users-guide/run_security.html) to setup a PSK.  
* VARNISH_STORAGE (default: `malloc,100m`): The cache backend and its configuration 

To use the geoip module, mount (or copy) the legacy database files (GeoIPASNum.dat, GeoIPASNumv6.dat, GeoIPCity.dat, GeoIP.dat, GeoIPv6.dat, GeoLiteCity.dat, GeoLiteCityv6.dat) to _/usr/share/GeoIP/_, e.g.

```
docker run --rm -it -p 127.0.0.1:8080:80 -v $PWD/test/legacy.vcl:/etc/varnish/default.vcl -v $PWD/geoip:/usr/share/GeoIP/ --name varnish emgag/varnish:4.1.11
```

# License

View [license information](https://github.com/emgag/docker-varnish/blob/master/LICENSE) for the software contained in this image.

## Issues

If you have any problems with or questions about this image, please contact us through a [GitHub issue](https://github.com/emgag/docker-varnish/issues).

## Contributing

You are invited to contribute new features, fixes, or updates, large or small; we are always thrilled to receive pull requests, and do our best to process them as fast as we can.

Before you start to code, we recommend discussing your plans through a [GitHub issue](https://github.com/emgag/docker-varnish/issues), especially for more ambitious contributions. This gives other contributors a chance to point you in the right direction, give you feedback on your design, and help you find out if someone else is working on the same thing.
