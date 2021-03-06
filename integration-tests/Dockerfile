# Systemd inside a Docker container, for CI only
FROM ubuntu:18.04

RUN apt-get update --yes

RUN apt-get install --yes systemd curl git sudo

# Kill all the things we don't need
RUN find /etc/systemd/system \
    /lib/systemd/system \
    -path '*.wants/*' \
    -not -name '*journald*' \
    -not -name '*systemd-tmpfiles*' \
    -not -name '*systemd-user-sessions*' \
    -exec rm \{} \;

RUN mkdir -p /etc/sudoers.d

RUN systemctl set-default multi-user.target

STOPSIGNAL SIGRTMIN+3

# Set up image to be useful out of the box for development & CI
ENV TLJH_BOOTSTRAP_DEV=yes
ENV TLJH_BOOTSTRAP_PIP_SPEC=/srv/src
ENV PATH=/opt/tljh/hub/bin:${PATH}

CMD ["/bin/bash", "-c", "exec /sbin/init --log-target=journal 3>&1"]

RUN mkdir -p /srv/src
ADD ./ /srv/src/
WORKDIR /srv/src
CMD ["ls","-la"]
RUN python3 /srv/src/bootstrap/bootstrap.py --admin admin