FROM ubuntu:20.04

# Install deps
RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive \
    apt-get install -y --no-install-recommends \
    apache2 \
    bash \
    bison \
    camlp4 \
    default-jdk-headless \
    flex \
    graphviz \
    libfl-dev \
    lttoolbox \
    g++ \
    gcc \
    git \
    make \
    ocaml \
    ocamlbuild \
    perl \
    python3 \
    python3-pip \
    python3-pandas \
    python3-openpyxl \
    xsltproc \
    && pip3 install anytree

SHELL ["/bin/bash", "-c"]

WORKDIR /app

# Copy SPEC file
COPY spec.txt ./

RUN a2enmod cgid && echo "ServerName localhost" >> /etc/apache2/apache2.conf

# Setup ZEN
RUN set -eux; \
    SPEC_FILE="spec.txt"; \
    source "$SPEC_FILE"; \
    git clone --depth 1 https://gitlab.inria.fr/huet/Zen.git "$ZENINSTALLDIR"; \
    cd "$ZENINSTALLDIR/ML" && make;

# Copy entrypoint script
COPY --chmod=755 entrypoint.sh ./

# Expose Apache server port
EXPOSE 80

# Run Apache server when container starts
CMD ["./entrypoint.sh"]
