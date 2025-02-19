FROM ubuntu:jammy # ubuntu 22.04 - docker pull ubuntu:jammy

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
    apt-get install --no-install-recommends -y ca-certificates build-essential checkinstall zlib1g-dev wget

# Update system packages
RUN apt-get update && apt-get upgrade -y

# Download and extract OpenSSL
RUN cd /usr/local/src/ && \
    wget https://www.openssl.org/source/openssl-3.0.9.tar.gz && \
    tar -xf openssl-3.0.9.tar.gz

# Configure, build, and install OpenSSL
WORKDIR /usr/local/src/openssl-3.0.9
RUN ./config --prefix=/usr/local/ssl --openssldir=/usr/local/ssl --libdir=/usr/local/ssl/lib shared zlib enable-fips && \
    make && \
    make install

# Configure library paths
RUN echo "/usr/local/ssl/lib" > /etc/ld.so.conf.d/openssl-3.0.9.conf && \
    ldconfig

# Create symlinks
RUN ln -sf /usr/local/ssl/bin/openssl /usr/bin/openssl && \
    ln -sf /usr/local/ssl/lib/libssl.so.3 /usr/lib/libssl.so.3 && \
    ln -sf /usr/local/ssl/lib/libcrypto.so.3 /usr/lib/libcrypto.so.3

# Create FIPS module directory
RUN mkdir -p /usr/local/ssl/lib/ossl-modules && \
    if [ -f /usr/local/ssl/lib/fips.so ]; then \
        ln -sf /usr/local/ssl/lib/fips.so /usr/local/ssl/lib/ossl-modules/fips.so; \
    else \
        echo "fips.so not found!"; \
    fi

# Create OpenSSL configuration file
COPY openssl.cnf /usr/local/ssl/openssl.cnf

# Install additional packages
#RUN apt-get install -y libgcrypt20 libgcrypt20-hmac strongswan strongswan-hmac openssh-client openssh-server
RUN apt-get install -y libgcrypt20 strongswan openssh-client openssh-server

# Detach Pro subscription and clean up
RUN apt-get purge --auto-remove -y ubuntu-pro-client ca-certificates build-essential checkinstall zlib1g-dev wget && \
    rm -rf /var/lib/apt/lists/* /usr/local/src/openssl-3.0.9*

ENV OPENSSL_CONF=/usr/local/ssl/openssl.cnf
ENV OPENSSL_MODULES=/usr/local/ssl/lib/ossl-modules
ENV PATH=/usr/local/ssl/bin:$PATH

