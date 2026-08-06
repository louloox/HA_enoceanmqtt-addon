ARG BUILD_FROM
FROM $BUILD_FROM

ARG BUILD_VERSION

# Pinned commits of the louloox forks (reproducible build).
# Bump these when you intentionally move the forks forward, then rebuild the add-on.
ARG ENOCEAN_SHA=088b91866234dcff3d90fec79cb761716b39aa67
ARG HA_ENOCEANMQTT_SHA=0aa4dbda391adff01f4376c4b6688ea52bb4ea79

# Create Working Directory
WORKDIR /app

# Install enoceanmqtt and requirements
RUN apk add --no-cache python3 py3-pip git && \
    python3 -m venv /app/venv && \
    . /app/venv/bin/activate && \
    python3 -m pip install --upgrade pip && \
    pip3 install pyyaml && \
    pip3 install tinydb && \
    pip3 install setuptools && \
    pip3 install paho-mqtt==1.6.1 && \
    pip3 install git+https://github.com/louloox/enocean.git@${ENOCEAN_SHA} && \
    git clone -b master --single-branch --depth 1 https://github.com/embyt/enocean-mqtt.git && \
    cd enocean-mqtt && python3 setup.py develop && cd .. && \
    git clone https://github.com/louloox/HA_enoceanmqtt.git && \
    cd HA_enoceanmqtt && git checkout ${HA_ENOCEANMQTT_SHA} && cd .. && \
    cp -rf HA_enoceanmqtt/enoceanmqtt enocean-mqtt && \
    rm -rf HA_enoceanmqtt

# Copy data for add-on
COPY run.sh .
RUN chmod a+x run.sh

CMD [ "/app/run.sh" ]
