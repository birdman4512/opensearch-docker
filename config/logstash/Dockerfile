# Declare args
ARG LOGSTASH_VERSION

FROM docker.elastic.co/logstash/logstash-oss:${LOGSTASH_VERSION:-9.0.1}

RUN bin/logstash-plugin install logstash-output-opensearch
	
	 