FROM ubuntu:16.04
RUN apt update && apt install -y --no-install-recommends \
      ca-certificates \
      openssh-server \
      sudo \
      apt-cacher-ng

RUN useradd --create-home --user-group vagrant && \
    echo -n 'vagrant:vagrant' | chpasswd && \
    echo 'vagrant ALL=NOPASSWD: ALL' >/etc/sudoers.d/vagrant

VOLUME "/var/cache/apt-cacher-ng"

RUN mkdir -p /var/run/apt-cacher-ng
CMD /usr/sbin/apt-cacher-ng -c /etc/apt-cacher-ng foreground=1
