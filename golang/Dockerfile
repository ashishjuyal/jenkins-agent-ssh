FROM golang:1.16-stretch
LABEL maintainer="Ashish Juyal <ashish@thecodecamp.in>"

RUN apt-get update && \    
    apt-get install -qy git && \
# Install a basic SSH server
    apt-get install -qy openssh-server && \
    sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd && \
    mkdir -p /var/run/sshd && \
#Install JDK 11 [fixing installation bug by creating below folder https://github.com/puckel/docker-airflow/issues/182#issuecomment-444683455]
	mkdir -p /usr/share/man/man1 && \
    apt-get install -qy openjdk-8-jre ansible && \
# Cleanup old packages
    apt-get -qy autoremove && \
# Add user jenkins to the image
    useradd -m jenkins && \
    mkdir /home/jenkins/.ssh && \
    mkdir -p /home/jenkins/go/src && \
    mkdir -p /home/jenkins/go/bin && \
# Set password for the jenkins user (you may want to alter this).
    echo "jenkins:jenkins" | chpasswd
# Copy authorized keys
COPY .ssh/authorized_keys /home/jenkins/.ssh/authorized_keys
COPY environment /etc/environment

RUN chown -R jenkins:jenkins /home/jenkins/.ssh/
RUN chown -R jenkins:jenkins /home/jenkins/go

# Standard SSH port
EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
