# Zoneminder with Event Notification Server Dockerfile
I needed a docker container with [Zoneminder](https://github.com/ZoneMinder/zoneminder) and the [Event Notification Server](https://github.com/pliablepixels/zmeventnotification) and this is the result.  Derived from the zoneminder official images.

No encryption is included.

# Build
docker-compose build

# Configure
Update the zmeventnotification.ini

# Run
docker-compose up -d
