FROM zharaskhan/dmoj-base:latest

# Install Pandoc
RUN wget https://github.com/jgm/pandoc/releases/download/3.1.8/pandoc-3.1.8-1-amd64.deb && \
    dpkg -i pandoc-3.1.8-1-amd64.deb && \
    rm pandoc-3.1.8-1-amd64.deb

ADD repo /site/
