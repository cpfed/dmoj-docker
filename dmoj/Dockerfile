FROM python:3.11-slim-bookworm

RUN apt-get update && \
    apt-get install -y \
        git gcc g++ make curl gettext wget \
        libxml2-dev libxslt1-dev zlib1g-dev \
        mariadb-client libmariadb-dev \
        libjpeg-dev debconf-utils pkg-config && \
    curl -sL https://deb.nodesource.com/setup_16.x | bash - && \
    apt-get install -y nodejs && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/*

RUN npm install -g sass postcss-cli postcss autoprefixer
RUN pip3 install \
        pymysql mysqlclient websocket-client uwsgi django-redis redis

COPY repo/requirements.txt .
RUN pip3 install -r requirements.txt

# Install Pandoc
RUN if [ "$(arch)" = "aarch64" ]; then \
    wget https://github.com/jgm/pandoc/releases/download/3.1.8/pandoc-3.1.8-1-arm64.deb && \
    dpkg -i pandoc-3.1.8-1-arm64.deb && \
    rm pandoc-3.1.8-1-arm64.deb; \
else \
    wget https://github.com/jgm/pandoc/releases/download/3.1.8/pandoc-3.1.8-1-amd64.deb && \
    dpkg -i pandoc-3.1.8-1-amd64.deb && \
    rm pandoc-3.1.8-1-amd64.deb; \
fi
