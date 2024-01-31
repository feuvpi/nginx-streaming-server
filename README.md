# Nginx Streaming Server

This repository contains the configuration files and setup instructions for running an Nginx streaming server using Docker.

## Prerequisites

Before you begin, ensure you have Docker installed on your system.

## Setup Instructions

1. Clone this repository to your local machine:

   ```bash
   git clone <repository-url>
   ```

2. Navigate to the cloned directory:

   ```bash
   cd nginx-streaming-server
   ```

3. Customize the `nginx.conf` file according to your requirements. This file contains the Nginx configuration for the streaming server.

4. Build the Docker container:

   ```bash
   docker-compose up --build
   ```

5. Once the container is up and running, you can start streaming to it.

## Configuration Details

### Nginx Configuration

The `nginx.conf` file contains the following configuration:

```nginx
worker_processes auto;

events { 
    worker_connections 1024;
}

rtmp {
    server{
        listen 1935;

        application live {
            live on;
            record off;
            hls on;
            hls_path /hls/live;
            hls_fragment 3;
        }
    }
}

http {
    server {
        listen 8081;

        location /live{
            types {
                application/vnd.apple.mpegurl m3u8;
                video/mp2t;
            }

            root /hls;
        }
    }
}
```

### Docker Configuration

The `docker-compose.yml` file defines the Docker service for Nginx with the following specifications:

```yaml
services:
  nginx:
    image: tiangolo/nginx-rtmp:latest
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - /stream:/hls/live
    ports:
      - "1935:1935"
      - "8081:8081"
```

This configuration exposes ports `1935` for RTMP streaming and `8081` for HTTP streaming.

### Streaming Configuration

- RTMP streaming is configured to listen on port `1935` with the `live` application enabled.
- HLS streaming is enabled with a fragment duration of `3` seconds.
- HTTP streaming is configured to listen on port `8081` and serves HLS streams from the `/hls` directory.

## Usage

Once the Nginx server is up and running, you can start streaming your content to it. 

For RTMP streaming, use the configured RTMP URL and key in your streaming software.

For HTTP streaming, access the HLS stream via the provided URL.



