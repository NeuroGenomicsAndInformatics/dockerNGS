FROM openjdk:8-jre
LABEL maintainer="Achal Neupane <achal@wustl.edu>"

LABEL org.label-schema.schema-version="1.0"
# LABEL org.label-schema.build-date=$BUILD_DATE
LABEL org.label-schema.name="achalneupane/cram2bam"
LABEL org.label-schema.description="Image for Reverting .bam or .cram"

ENV PICARD_VERSION 2.20.8
ENV SAMTOOLS_VERSION 1.9

WORKDIR /tmp

RUN apt-get update -y \
  && apt-get install --no-install-recommends -y \
       make \
       gcc \
       g++ \
       libz-dev \
       libbz2-dev \
       liblzma-dev \
       ncurses-dev \
       bc \
       libnss-sss \
       time \
  && cd /tmp \
  && wget -q https://github.com/samtools/samtools/releases/download/${SAMTOOLS_VERSION}/samtools-${SAMTOOLS_VERSION}.tar.bz2 \
  && tar xjvf samtools-${SAMTOOLS_VERSION}.tar.bz2 \
  && cd /tmp/samtools-${SAMTOOLS_VERSION}/ \
  && make \
  && cp -av /tmp/samtools-${SAMTOOLS_VERSION}/samtools /usr/bin/ \
  && wget -q -O /usr/bin/picard.jar https://github.com/broadinstitute/picard/releases/download/${PICARD_VERSION}/picard.jar \
  && ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime \
  && echo "America/Chicago" > /etc/timezone \
  && dpkg-reconfigure --frontend noninteractive tzdata \
  && apt-get clean all \
  && rm -rfv /var/lib/apt/lists/* /tmp/* /var/tmp/* 

# This makes the image crazy large -- will find a workaround
# COPY human_g1k_v37_decoy* /usr/local/

COPY ./entrypoint.sh /usr/local/bin/
ENV PICARD /usr/bin/picard.jar

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
# CMD ["/bin/bash"]