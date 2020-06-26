FROM evarga/jenkins-slave
# install docker and chefdk
ARG CHANNEL=stable
ARG VERSION=2.3.3
ARG USER=jenkins
ARG USER_ID=1000
ARG GROUP_ID=1000
ENV DEBIAN_FRONTEND=noninteractive \
    PATH=/opt/chefdk/bin:/opt/chefdk/embedded/bin:/root/.chefdk/gem/ruby/2.4.0/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
# prereqs
RUN apt-get update && \
    apt-get install -y apt-transport-https ca-certificates curl gpg-agent software-properties-common wget ssh
# install docker
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - && \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" && \
    apt-get update && \
    apt-get install -y docker-ce
RUN wget --quiet --content-disposition "http://packages.chef.io/files/${CHANNEL}/chefdk/${VERSION}/ubuntu/16.04/chefdk_${VERSION}-1_amd64.deb" -O /tmp/chefdk.deb && \
    dpkg -i /tmp/chefdk.deb && \
    chef gem install kitchen-docker && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    groupmod -g ${GROUP_ID} jenkins && \
    usermod -u ${USER_ID} -g ${GROUP_ID} ${USER} && \
    adduser ${USER} docker
USER jenkins
VOLUME /var/run/docker.sock
