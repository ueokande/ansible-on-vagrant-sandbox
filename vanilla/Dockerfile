FROM ubuntu:16.04

RUN apt update && apt install -y --no-install-recommends \
      openssh-server \
      sudo \
      ca-certificates \
      apt-transport-https \
      curl

RUN useradd --create-home --user-group vagrant && \
    echo -n 'vagrant:vagrant' | chpasswd && \
    echo 'vagrant ALL=NOPASSWD: ALL' >/etc/sudoers.d/vagrant

RUN echo 'Acquire::http::Proxy "http://apt-cacher:3142/";' >/etc/apt/apt.conf.d/02proxy

RUN mkdir -p /var/run/sshd
CMD /usr/sbin/sshd -D
