# Combination of https://github.com/DMOJ/judge-server/blob/master/.docker/tier1/Dockerfile
# and https://github.com/DMOJ/runtimes-docker/blob/master/tier1/Dockerfile

# Only Python 3 and GCC (C/C++) are installed.

FROM debian:sid

ARG TAG=master

RUN echo deb http://deb.debian.org/debian/ bullseye main > /etc/apt/sources.list.d/bullseye.list && \
    echo deb http://security.debian.org/debian-security bullseye-security main >> /etc/apt/sources.list.d/bullseye.list && \
    echo deb http://deb.debian.org/debian/ experimental main > /etc/apt/sources.list.d/experimental.list && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        curl file gcc g++ python3-full python3-pip python3-dev python3-setuptools python3-wheel cython3 libseccomp-dev bzip2 gzip \
        libxtst6 tini \
        $([ "$(arch)" = aarch64 ] && echo binutils-arm-linux-gnueabihf) && \
    mkdir -p /etc/perl && \
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    useradd -m judge

RUN mkdir /judge /problems && cd /judge && \
	curl -L https://github.com/DMOJ/judge-server/archive/"${TAG}".tar.gz | tar -xz --strip-components=1 && \
    python3 -m venv --prompt=DMOJ /env && \
	/env/bin/pip3 install cython && \
	/env/bin/pip3 install -e . && \
	/env/bin/python3 setup.py develop && \
	HOME=~judge . ~judge/.profile && \
	runuser -u judge -w PATH -- /env/bin/dmoj-autoconf -V > /judge-runtime-paths.yml && \
	echo '  crt_x86_in_lib32: true' >> /judge-runtime-paths.yml

ENTRYPOINT ["/usr/bin/tini", "--", "/judge/.docker/entry"]
