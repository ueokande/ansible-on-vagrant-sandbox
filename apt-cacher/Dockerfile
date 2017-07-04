FROM ubuntu:16.04
RUN apt update && apt install -y --no-install-recommends \
      ca-certificates \
      openssh-server \
      sudo \
      apt-cacher-ng

VOLUME "/var/cache/apt-cacher-ng"

RUN mkdir -p /var/run/apt-cacher-ng
CMD /usr/sbin/apt-cacher-ng -c /etc/apt-cacher-ng foreground=1
