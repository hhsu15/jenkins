FROM centos

# ssh server for centos
RUN yum -y install openssh-server

# create user, set password for the user and a directory
RUN useradd remote_user && \
    echo "remote_user:1234" | chpasswd  && \
    mkdir /home/remote_user/.ssh && \
    chmod 700 /home/remote_user/.ssh

# copy key to directory
COPY remote-key.pub /home/remote_user/.ssh/authorized_keys

# change owner. everything under the folder is going to be owned by this user
RUN chown remote_user:remote_user -R /home/remote_user/.ssh/&& \
    chmod 600 /home/remote_user/.ssh/authorized_keys

RUN ssh-keygen -A 
RUN rm -rf /run/nologin
# command for docker to start as a service
CMD /usr/sbin/sshd -D
