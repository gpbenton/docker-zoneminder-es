FROM ubuntu:18.04
MAINTAINER Peter Gallagher

# Update base packages
RUN apt update \
    && apt upgrade --assume-yes

# Install pre-reqs
RUN apt install --assume-yes --no-install-recommends gnupg

# Configure Zoneminder PPA
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys ABE4C7F993453843F0AEB8154D0BF748776FFB04 \
    && echo deb http://ppa.launchpad.net/iconnor/zoneminder-1.34/ubuntu bionic main > /etc/apt/sources.list.d/zoneminder.list \
    && apt update

# Install zoneminder
RUN DEBIAN_FRONTEND=noninteractive apt install --assume-yes zoneminder \
    && a2enconf zoneminder \
    && a2enmod rewrite cgi

# Setup Volumes
VOLUME /var/cache/zoneminder/events /var/cache/zoneminder/images /var/lib/mysql /var/log/zm

# Expose http port
EXPOSE 80

# Configure entrypoint
COPY utils/entrypoint.sh /usr/local/bin/
RUN chmod 755 /usr/local/bin/entrypoint.sh
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]

# Configure zmeventnotification
WORKDIR /root
RUN apt-get install -y git libyaml-perl make libjson-perl gcc
RUN git clone https://github.com/pliablepixels/zmeventnotification.git
WORKDIR /root/zmeventnotification
RUN git fetch --tags
RUN git checkout $(git describe --tags $(git rev-list --tags --max-count=1))
EXPOSE 9000
RUN perl -MCPAN -e "install Crypt::MySQL"
RUN perl -MCPAN -e "install Config::IniFiles"
RUN perl -MCPAN -e "install Crypt::Eksblowfish::Bcrypt"
RUN perl -MCPAN -e "install Net::WebSocket::Server"
RUN perl -MCPAN -e "install LWP::Protocol::https"
RUN perl -MCPAN -e "install Net::MQTT::Simple"
RUN chmod a+x ./zmeventnotification.pl

RUN ./install.sh --no-interactive --install-es --no-install-hook --install-config --no-pysudo

COPY utils/csresnext50-panet-spp-original-optimal_final.weights /var/lib/zmeventnotification/models/cspn
