ARG gdb_version
FROM ontotext/graphdb:${gdb_version}
ARG st_version
RUN set -eux ; \
    wget -P /opt/graphdb/dist/lib "https://search.maven.org/remotecontent?filepath=it/uniroma2/art/semanticturkey/st-trivial-inference-sail/${st_version}/st-trivial-inference-sail-${st_version}.jar" ; \
	wget -P /opt/graphdb/dist/lib "https://search.maven.org/remotecontent?filepath=it/uniroma2/art/semanticturkey/st-changetracking-sail/${st_version}/st-changetracking-sail-${st_version}.jar"
