# NGINX GeoIP2 Load Balancing

## Servers

```bash
# Static upstream servers default.
upstream geo_default {
    server 127.0.0.1:8055 max_fails=1 fail_timeout=5s;
    server 127.0.0.1:8059 backup;
}

# Static upstream servers for UK.
upstream geo_UK {
    server 127.0.0.1:8056 max_fails=1 fail_timeout=5s;
    server 127.0.0.1:8059 backup;
}

# Static upstream servers for US.
upstream geo_US {
    server 127.0.0.1:8057 max_fails=1 fail_timeout=5s;
    server 127.0.0.1:8058 max_fails=1 fail_timeout=5s;
    server 127.0.0.1:8059 backup;
}
```

## Backup servers

The same backup server for each upstream.

```bash
server 127.0.0.1:8059 backup;
```

## Health check

Mark server as a `down` after 1 fail and check if active every 5 seconds.

```bash
max_fails=1 fail_timeout=5s;
```

## Examples

Tests were done using `NGROK`.

![ngrok.](./example_images/ngrok.png)

  * Nginx default upstream(local IP).
    ![Nginx default upstream.](./example_images/nginx_default_upstream.png)

  * Nginx UK upstream(United Kingdom IP).
    ![Nginx uk upstream.](./example_images/nginx_uk_upstream.png)

  * Nginx US upstream(United States IP).
    ![Nginx us 1 upstream.](./example_images/nginx_us1_upstream.png)
    
    ![Nginx us 2 upstream.](./example_images/nginx_us2_upstream.png)

  * Nginx backup server.
    ```bash
    # Static upstream servers default.
    upstream geo_default {
        server 127.0.0.1:8055 max_fails=1 fail_timeout=5s down;
        server 127.0.0.1:8059 backup;
    }
    ```

    ![Nginx backup server.](./example_images/nginx_backup_server.png)