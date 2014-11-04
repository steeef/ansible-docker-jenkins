FROM centos:centos6
MAINTAINER Stephen Price <stephen@stp5.net>

RUN rpm -Uvh --force http://mirrors.kernel.org/fedora-epel/6/x86_64/epel-release-6-8.noarch.rpm \
    && yum -y install openssh openssh-server sudo passwd python-simplejson \
    && yum clean all

RUN ssh-keygen -q -N "" -t dsa -f /etc/ssh/ssh_host_dsa_key \
    && ssh-keygen -q -N "" -t rsa -f /etc/ssh/ssh_host_rsa_key \
    && sed -ri 's/^UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config \
    && sed -ri 's/^#UsePAM no/UsePAM no/g' /etc/ssh/sshd_config \
    && sed -ri 's/^GSSAPIAuthentication yes/GSSAPIAuthentication no/g' /etc/ssh/sshd_config \
    && sed -ri 's/^#UseDNS yes/UseDNS no/g' /etc/ssh/sshd_config \
    && sed -ri 's/^#UsePrivilegeSeparation yes/UsePrivilegeSeparation no/g' /etc/ssh/sshd_config \
    && sed -ri 's/^PasswordAuthentication yes/PasswordAuthentication no/g' /etc/ssh/sshd_config

COPY id_ansible.pub /home/ansible/.ssh/authorized_keys

RUN useradd ansible \
    && chown -R ansible. /home/ansible \
    && chmod 700 /home/ansible/.ssh \
    && chmod 600 /home/ansible/.ssh/authorized_keys \
    && echo 'ansible ALL=NOPASSWD: ALL' >>/etc/sudoers \
    && passwd -f -u ansible

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
