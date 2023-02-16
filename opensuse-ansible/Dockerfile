FROM opensuse/leap
LABEL maintainer="Alex Kraker"
ENV container=docker

# Install requirements
RUN zypper refresh && zypper update -y
RUN zypper install -y \
which \
sudo \
openssh \
python3 \
python3-pip


# openSUSE systemd base image
# See: https://github.com/robertdebock/docker-opensuse-systemd
RUN zypper install -y dbus-1 systemd-sysvinit
RUN cd /usr/lib/systemd/system/sysinit.target.wants; \
for i in *; do \
[ $i = systemd-tmpfiles-setup.service ] || rm -f $i; \
done;
RUN rm -f \
/usr/lib/systemd/system/multi-user.target.wants/* \
/etc/systemd/system/*.wants/* \
/usr/lib/systemd/system/local-fs.target.wants/* \
/usr/lib/systemd/system/sockets.target.wants/*udev* \
/usr/lib/systemd/system/sockets.target.wants/*initctl* \
/usr/lib/systemd/system/basic.target.wants/* \
/usr/lib/systemd/system/anaconda.target.wants/*

VOLUME ["/sys/fs/cgroup"]
CMD ["/sbin/init"]