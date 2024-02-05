FROM eclipse-temurin:21-alpine

ARG vb_version=12.1.0
ARG st_version=12.1

COPY vocbench3-${vb_version}-full.zip /tmp
RUN set -eux ; \
	mkdir /opt/vocbench3 ; \
	mkdir /opt/vocbench3/data ; \
	cd /opt/vocbench3 ; \
	unzip -q /tmp/vocbench3-${vb_version}-full.zip -d . ; \
	chmod -R u=rwX,go=rX semanticturkey-${st_version} ; \
	chmod +x semanticturkey-${st_version}/bin/* ; \
	echo 'semanticturkey.data.dir=../data/SemanticTurkeyData' > semanticturkey-${st_version}/application.properties ; \
	rm /tmp/vocbench3-${vb_version}-full.zip

WORKDIR /opt/vocbench3/semanticturkey-$st_version

EXPOSE 1979

ENTRYPOINT ["bin/semanticturkey.sh"]