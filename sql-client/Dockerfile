FROM openjdk:11-jre

RUN curl https://dlcdn.apache.org/flink/flink-1.16.3/flink-1.16.3-bin-scala_2.12.tgz -O && \
    tar -zxf flink-1.16.3-bin-scala_2.12.tgz -C /opt && \
    rm -f flink-1.16.3-bin-scala_2.12.tgz

WORKDIR /opt/flink-1.16.3

RUN curl https://repo1.maven.org/maven2/org/apache/iceberg/iceberg-flink-runtime-1.17/1.3.0/iceberg-flink-runtime-1.17-1.3.0.jar -o lib/iceberg-flink-runtime-1.17-1.3.0.jar && \
    curl https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-common/2.8.3/hadoop-common-2.8.3.jar -o lib/hadoop-common-2.8.3.jar && \
    curl https://repo.maven.apache.org/maven2/org/apache/flink/flink-shaded-hadoop-2-uber/2.8.3-10.0/flink-shaded-hadoop-2-uber-2.8.3-10.0.jar -o lib/flink-shaded-hadoop-2-uber-2.8.3-10.0.jar && \
    curl https://repo1.maven.org/maven2/software/amazon/awssdk/bundle/2.20.18/bundle-2.20.18.jar -o lib/bundle-2.20.18.jar

COPY conf/flink-conf.yaml conf/flink-conf.yaml


