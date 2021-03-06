FROM ubuntu:bionic
COPY auto-respond.txt auto-respond.txt
ENV CLOUD_SDK_VERSION 206.0.0

ARG INSTALL_COMPONENTS
RUN apt-get update -qqy && apt-get install -qqy \
        curl \
        apt-transport-https \
        lsb-release \
        openssh-client \
        git \
        unzip \
        zip \
    && easy_install -U pip && \
    pip install -U crcmod && \
    export CLOUD_SDK_REPO="cloud-sdk-$(lsb_release -c -s)" && \
    echo "deb https://packages.cloud.google.com/apt $CLOUD_SDK_REPO main" > /etc/apt/sources.list.d/google-cloud-sdk.list && \
    curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add - && \
    apt-get update && apt-get install -y google-cloud-sdk=${CLOUD_SDK_VERSION}-0 $INSTALL_COMPONENTS && \
    gcloud config set core/disable_usage_reporting true && \
    gcloud config set component_manager/disable_update_check true && \
    gcloud config set metrics/environment github_docker_image && \
    gcloud --version && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    wget https://installbuilder.bitrock.com/installbuilder-enterprise-18.5.2-linux-x64-installer.run && \
    chmod +x installbuilder-enterprise-18.5.2-linux-x64-installer.run && \
    cat auto-respond.txt | ./installbuilder-enterprise-18.5.2-linux-x64-installer.run && \
    rm installbuilder-enterprise-18.5.2-linux-x64-installer.run && rm Release.key && \
    mkdir -p home && \
    ln -s opt/installbuilder-18.5.2/output/* /home/

ENV PATH "$PATH:/opt/installbuilder-18.5.2/bin/"
WORKDIR /home
VOLUME [ "/home/" ]

CMD [ "/bin/bash" ]