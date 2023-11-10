# borrowed from https://github.com/Ontotext-AD/graphdb-docker/tree/master
# to alter the base image from eclipse-temurin:11-jdk-alpine to eclipse-temurin:21-jdk-alpine


FROM eclipse-temurin:21-jdk-alpine

# Build time arguments
ARG version=10.4.1
ARG st_version
ARG vb_version
ENV GRAPHDB_PARENT_DIR=/opt/graphdb
ENV GRAPHDB_HOME=${GRAPHDB_PARENT_DIR}/home

ENV GRAPHDB_INSTALL_DIR=${GRAPHDB_PARENT_DIR}/dist

WORKDIR /tmp

COPY vocbench3-${vb_version}-full.zip .
COPY lucene-fts-plugin-graphdb-plugin.zip .

RUN apk add --no-cache bash curl util-linux procps net-tools busybox-extras wget less libc6-compat && \
    apk upgrade libssl3 libcrypto3 && \
    curl -fsSL "https://maven.ontotext.com/repository/owlim-releases/com/ontotext/graphdb/graphdb/${version}/graphdb-${version}-dist.zip" > \
    graphdb-${version}.zip && \
    bash -c 'md5sum -c - <<<"$(curl -fsSL https://maven.ontotext.com/repository/owlim-releases/com/ontotext/graphdb/graphdb/${version}/graphdb-${version}-dist.zip.md5)  graphdb-${version}.zip"' && \
    mkdir -p ${GRAPHDB_PARENT_DIR} && \
    cd ${GRAPHDB_PARENT_DIR} && \
    unzip /tmp/graphdb-${version}.zip && \
    rm /tmp/graphdb-${version}.zip && \
    mv graphdb-${version} dist && \
    mkdir -p ${GRAPHDB_HOME} && \
    ln -s /lib/libc.musl-x86_64.so.1 /lib/ld-linux-x86-64.so.2 && \
    unzip -j "/tmp/vocbench3-${vb_version}-full.zip" "semanticturkey-${st_version}/lib/st-changetracking-sail-${st_version}.jar" \ 
                                                "semanticturkey-${st_version}/lib/st-trivial-inference-sail-${st_version}.jar" \ 
                                                -d /opt/graphdb/dist/lib && \
    unzip -q "/tmp/lucene-fts-plugin-graphdb-plugin.zip" -d "/opt/graphdb/dist/lib/plugins" && \
	rm /tmp/vocbench3-${vb_version}-full.zip && \
    rm /tmp/lucene-fts-plugin-graphdb-plugin.zip

ENV PATH=${GRAPHDB_INSTALL_DIR}/bin:$PATH

CMD ["-Dgraphdb.home=/opt/graphdb/home"]

ENTRYPOINT ["/opt/graphdb/dist/bin/graphdb"]

EXPOSE 7200
EXPOSE 7300