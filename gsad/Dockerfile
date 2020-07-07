FROM ubuntu:focal
LABEL maintainer="Richard Phillis <rphillis@studiosity.com>"
# LABEL maintainer="Mohammad Razavi <mrazavi64@gmail.com>"

RUN set -ex; \
    apt-get update; \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends gnupg; \
    # 3C453D244AA450E0: "Launchpad PPA for Mohammad Razavi"
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3C453D244AA450E0; \
    echo "deb http://ppa.launchpad.net/mrazavi/gvm/ubuntu focal main" > /etc/apt/sources.list.d/mrazavi-ubuntu-gvm-focal.list; \
    apt-get update; \
    DEBIAN_FRONTEND=noninteractive apt-get install -y gsad ; \
    sed -i 's|/var/log/gvm/gsad.log|/dev/stdout|g' /etc/gvm/gsad_log.conf; \
    rm -rf /var/lib/apt/lists/*

ENV GVMD_HOST="gvmd" \
    GVMD_PORT="9390"

EXPOSE 80

COPY docker-entrypoint.sh /usr/local/bin/
ENTRYPOINT ["docker-entrypoint.sh"]

CMD ["gsad", "-f", "--listen=0.0.0.0", "--port=80", "--http-only", "--mlisten=$GVMD_HOST", "--mport=$GVMD_PORT"]
