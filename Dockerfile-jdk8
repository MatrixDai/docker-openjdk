FROM openjdk:8

# Default CPU core
ARG CPU_CORE=2

# Fix kill by OOM
# Config GC thread count equals CPU core
# Config Compiler thread count equals CPU core
ENV _JAVA_OPTIONS="-XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap -XX:ParallelGCThreads=${CPU_CORE} -XX:CICompilerCount=${CPU_CORE}"

ADD ./src/numcpus.c /tmp/

RUN DEBIAN_FRONTEND=noninteractive apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y gcc && \
    DEBIAN_FRONTEND=noninteractive apt-get clean all && \
    cd /tmp && \
    gcc -O3 -fPIC -shared -Wl,-soname,libnumcpus.so -o /usr/lib/libnumcpus.so numcpus.c && \
    chmod a+x /usr/lib/libnumcpus.so && \
    rm -rf /tmp/*

ENV LD_PRELOAD="/usr/lib/libnumcpus.so" \
    _NUM_CPUS="${CPU_CORE}"
