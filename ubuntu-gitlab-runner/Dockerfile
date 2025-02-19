# ubuntu 22.04 - docker pull ubuntu:jammy
FROM ubuntu:jammy 

ENV DEBIAN_FRONTEND=noninteractive
ENV LD_LIBRARY_PATH=/usr/local/ssl/lib

# Install required packages and attach Pro subscription
#RUN --mount=type=secret,id=pro-attach-config \
#    apt-get update && \
#    apt-get install --no-install-recommends -y ubuntu-pro-client ca-certificates build-essential checkinstall zlib1g-dev wget

# Attach Pro subscription
#RUN --mount=type=secret,id=pro-attach-config \
#    pro attach --attach-config /run/secrets/pro-attach-config


# Install required packages
RUN apt-get update && \
    apt-get install --no-install-recommends -y ca-certificates build-essential checkinstall zlib1g-dev wget openssl

# Update system packages
RUN apt-get update && apt-get upgrade -y


# Install additional packages
#RUN apt-get install -y libgcrypt20 libgcrypt20-hmac strongswan strongswan-hmac openssh-client openssh-server
RUN apt-get install -y libgcrypt20 strongswan openssh-client openssh-server

# Detach Pro subscription and clean up
RUN apt-get purge --auto-remove -y ubuntu-pro-client ca-certificates build-essential checkinstall zlib1g-dev wget && \
    rm -rf /var/lib/apt/lists/* /usr/local/src/openssl-3.0.9*

ENV OPENSSL_CONF=/usr/local/ssl/openssl.cnf
ENV OPENSSL_MODULES=/usr/local/ssl/lib/ossl-modules
ENV PATH=/usr/local/ssl/bin:$PATH

