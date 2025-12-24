FROM ubuntu:22.04

RUN apt-get update && apt-get install -y \
    build-essential \
    git \
    cmake \
    libssl-dev \
    libgflags-dev \
    libsnappy-dev \
    libzstd-dev \
    liblz4-dev \
    && rm -rf /var/lib/apt/lists/*

RUN git clone https://gitee.com/apache/kvrocks.git /kvrocks && \
    cd /kvrocks && \
    git checkout v2.1.0 && \
    mkdir build && \
    cd build && \
    cmake .. && \
    make -j$(nproc)

FROM ubuntu:22.04

RUN apt-get update && apt-get install -y \
    libssl3 \
    libgflags2.2 \
    libsnappy1v5 \
    libzstd1 \
    liblz4-1 \
    && rm -rf /var/lib/apt/lists/*

COPY --from=0 /kvrocks/build/src/kvrocks /usr/local/bin/
COPY --from=0 /kvrocks/conf /etc/kvrocks

EXPOSE 6666

CMD ["kvrocks", "-c", "/etc/kvrocks/kvrocks.conf"]
