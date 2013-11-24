Nginx Static Etags
------------------

This module provides support for generating (or swapping) Etag header for static content in nginx.

### Installation

Prepare location for core and/or module code:

    mkdir ${HOME}/src
    cd ${HOME}/src

Pull code by cloning the repositories and/or downloading packages:

    # download nginx sources and unpack
    curl http://nginx.org/download/nginx-1.4.4.tar.gz | tar xzf -
    # download module sources
    git clone https://github.com/soutys/nginx-static-etags.git ./nginx-static-etags

Compile module into nginx:

    cd ./nginx-1.4.4
    # add other options, modules, CFLAGS if you like...
    ./configure --add-module=${HOME}/src/nginx-static-etags
    # multithreaded compilation is also "supported" :)
    make -j4
    # use root privileges or define your own `--prefix=...` option for `configure` script
    sudo make install

That's (almost) all!

### Configuration

Update relevant `location` blocks in your `nginx.conf` file:

    location / {
        ...
        etags on;
        etag_hash on|off;
        etag_hash_method md5|sha1;
        ...
    }

### Optional pre/post-compilation tests

  * code checks:

    ```
    cppcheck --verbose --enable=all --inconclusive ${HOME}/src/ngx_http_static_etags_module.c
    valgrind --verbose --leak-check=full --show-reachable=yes --track-origins=yes /usr/local/nginx/sbin/nginx
    ```

  * debugging:

    * [no-pool nginx](https://github.com/shrimp/no-pool-nginx)
    * [nginx - Debugging](http://wiki.nginx.org/Debugging)
    * [nginx debug mode](http://www.bedis.eu/nginx/nginx_debug_mode)
    * [Day 36 - nginx debug and valgrind from Eclipse](http://www.nginx-discovery.com/2011/03/day-36-nginx-debug-and-valgrind-from.html)

### TODOs

 * bring it to feature parity with [the Apache configuration option][apache]

### Credits

 *  [Mike West](http://mikewest.org/)
 *  [Adrian Jung](http://me2day.net/kkung)

[apache]: http://httpd.apache.org/docs/1.3/mod/core.html#fileetag
