#### Supported tags and respective `Dockerfile` links

* `jtreminio/node-passenger:10`, `jtreminio/node-passenger:latest`, ([Dockerfile](https://github.com/jtreminio/node-passenger-docker/blob/master/Dockerfile-10))

#### [This README best viewed on Github for formatting](https://github.com/jtreminio/node-passenger-docker/blob/master/README.md)

## How to use this image

### With Command Line

For Node projects that do not require a web server (in local dev mode), you can do the following

#### Create a Dockerfile in your Node project

    FROM jtreminio/node-passenger:10
    COPY . /var/www
    WORKDIR /var/www
    CMD [ "yarn", "run" ]

Then, run the commands to build and run the Docker image:

    docker build -t my-node-app .
    
    docker run -it --rm --name my-running-app my-node-app

#### Run without Dockerfile

If you want to use this image without having to write a Dockerfile:

    docker run -it --rm \
        --name my-running-script \
        -v "$PWD":/var/www \
        -w /var/www \
        jtreminio/node-passenger:10 yarn run

## With Nginx

The images are based on `phusion/passenger-nodejs` which comes with Nginx installed.

The [default vhost config](https://github.com/jtreminio/node-passenger-docker/blob/master/files/vhost.conf) listens on port `8080`, expects your app's `public` directory to be located at `/var/www/public`.

By convention, expected entry point is `app.js`. If your app uses another file to start change `passenger_startup_file app.js;` to reflect your settings.

Simply run the following to get started:

    docker run -it --rm \
        --name my-running-script \
        -v "$PWD":/var/www \
        -w /var/www \
        jtreminio/node-passenger:10

Or use Docker Compose:

    version: '3.2'
    services:
      web:
        image: jtreminio/node-passenger:10
        ports:
          - 8080:8080
        volumes:
          - $PWD:/var/www

## About these images

Based on `phusion/passenger-nodejs`, these images update its installed Node to the latest minor version, and installs `yarn` as a global package.
