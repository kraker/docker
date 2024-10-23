# Adapted from @geerlingguy's: https://github.com/geerlingguy/docker-rockylinux9-ansible/
FROM rockylinux:9
LABEL maintainer="Alex Kraker"
ENV container=docker

ENV pip_packages "ansible"

# Install systemd -- See https://hub.docker.com/_/centos/
RUN rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

# Install requirements, dependencies, and Packer
RUN dnf -y install rpm dnf-plugins-core \
    && dnf config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo \
    && dnf -y update \
    && dnf -y install \
        epel-release \
        initscripts \
        sudo \
        which \
        hostname \
        libyaml \
        python3 \
        python3-pip \
        python3-pyyaml \
        iproute \
        packer \
        openssh \
        openssh-clients \
    && dnf clean all

# Unlink /usr/sbin/packer to avoid conflicts with Packer binary.
# https://github.com/cracklib/cracklib/issues/7
RUN unlink /usr/sbin/packer

# Upgrade pip to latest version.
RUN pip3 install --upgrade pip

# Install Ansible via Pip.
RUN pip3 install $pip_packages

# Disable requiretty.
RUN sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/'  /etc/sudoers

# Install Ansible inventory file.
RUN mkdir -p /etc/ansible
RUN echo -e '[local]\nlocalhost ansible_connection=local' > /etc/ansible/hosts

VOLUME ["/sys/fs/cgroup"]
CMD ["/usr/lib/systemd/systemd"]
