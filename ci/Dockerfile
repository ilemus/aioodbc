FROM python:3.8-slim

# configure apt to install minimal dependencies in non-interactive mode.
ENV DEBIAN_FRONTEND noninteractive
RUN echo 'APT::Install-Recommends "false";' >> /etc/apt/apt.conf; \
    echo 'APT::Install-Suggests "false";' >> /etc/apt/apt.conf

# Update apt cache and install ODBC core, drivers, build tools, and Python essentials
RUN apt-get update && apt-get install -y --no-install-recommends \
    # Core ODBC components
    unixodbc \
    unixodbc-dev \
    # Build tools (needed for some pip installs or if compiling other things)
    build-essential \
    # ODBC Drivers
    odbc-postgresql \
    # MariaDB ODBC driver (works for MySQL)
    odbc-mariadb \
    libsqliteodbc \
    # Utilities (if needed, e.g., for downloading other things)
    wget \
    curl \
    git \
    # -----------------------------------------------------------
    # Clean up APT cache to reduce image size
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

ADD . /aioodbc
WORKDIR /aioodbc

RUN pip install -e /aioodbc/ && \
    pip install -U pip setuptools && \
    pip install -r /aioodbc/requirements-dev.txt

# with --squash option in docker build, this will reduce the final image size a bit.
RUN rm -rf /aioodbc && \
    apt-get purge -y g++ && \
    apt-get purge -y wget && \
    apt-get autoremove -y

