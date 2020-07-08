FROM postgres:12
LABEL maintainer="Richard Phillis <rphillis@studiosity.com>"
# LABEL maintainer="Mohammad Razavi <mrazavi64@gmail.com>"

RUN set -ex; \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3B4FE6ACC0B21F32; \
    echo "deb http://archive.ubuntu.com/ubuntu focal main" > /etc/apt/sources.list.d/focal.list; \
    echo "deb http://archive.ubuntu.com/ubuntu focal-updates main" > /etc/apt/sources.list.d/focal-updates.list; \
    # 3C453D244AA450E0: "Launchpad PPA for Mohammad Razavi"
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3C453D244AA450E0; \
    echo "deb http://ppa.launchpad.net/mrazavi/gvm/ubuntu focal main" > /etc/apt/sources.list.d/mrazavi-ubuntu-gvm-focal.list; \
    apt-get update; \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends libgvm-pg-server0; \
    rm -rf /var/lib/apt/lists/*
