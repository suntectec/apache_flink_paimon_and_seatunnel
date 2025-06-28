FROM flink:1.16.3

ENV SEATUNNEL_VERSION="2.3.10"
ENV SEATUNNEL_HOME="/opt/seatunnel"

RUN wget https://archive.apache.org/dist/seatunnel/${SEATUNNEL_VERSION}/apache-seatunnel-${SEATUNNEL_VERSION}-bin.tar.gz && \
    tar -xzvf apache-seatunnel-${SEATUNNEL_VERSION}-bin.tar.gz && \
    mv apache-seatunnel-${SEATUNNEL_VERSION} ${SEATUNNEL_HOME}

RUN cd ${SEATUNNEL_HOME} || sh bin/install-plugin.sh ${SEATUNNEL_VERSION}

# 变更以后各层的工作目录的位置
WORKDIR ${SEATUNNEL_HOME}