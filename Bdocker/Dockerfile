FROM registry.access.redhat.com/ubi9/ubi:latest

LABEL maintainer="you@example.com"
LABEL description="Standalone DevOps Container with Jenkins, Terraform, and Ansible"

# Install system dependencies
RUN dnf install -y \
    git \
    wget \
    unzip \
    python3.11 \
    python3.11-pip \
    java-11-openjdk \
 && alternatives --install /usr/bin/python3 python3 /usr/bin/python3.11 1 \
 && alternatives --install /usr/bin/pip3 pip3 /usr/bin/pip3.11 1 \
 && dnf clean all

# Upgrade pip and install Ansible (latest stable full package)
RUN pip3 install --upgrade pip \
 && pip3 install ansible==8.7.0

# Install latest Terraform
ENV TERRAFORM_VERSION=1.7.5
RUN wget https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip \
 && unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip \
 && mv terraform /usr/local/bin/ \
 && rm -f terraform_${TERRAFORM_VERSION}_linux_amd64.zip

# Install Jenkins (latest stable WAR)
ENV JENKINS_VERSION=2.452.1
RUN mkdir -p /opt/jenkins && cd /opt/jenkins \
 && wget https://get.jenkins.io/war-stable/${JENKINS_VERSION}/jenkins.war

# Create Jenkins home directory
RUN mkdir -p /var/jenkins_home
ENV JENKINS_HOME=/var/jenkins_home

# Expose default Jenkins port
EXPOSE 8080

# Start Jenkins
CMD ["java", "-jar", "/opt/jenkins/jenkins.war"]