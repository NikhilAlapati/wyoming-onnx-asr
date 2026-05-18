ARG BASE_IMAGE=nvidia/cuda:13.0.1-runtime-ubuntu24.04
FROM ${BASE_IMAGE}

ARG CPU=0

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        python3 python3-pip ca-certificates && \
    rm -rf /var/lib/apt/lists/* && \
    ln -sf /usr/bin/python3 /usr/local/bin/python && \
    python3 -m pip install --upgrade pip setuptools wheel

WORKDIR /opt/wyoming-onnx-asr
COPY . /opt/wyoming-onnx-asr

RUN if [ "${CPU}" = "1" ]; then \
        python3 -m pip install -r requirements-cpu.txt; \
    else \
        python3 -m pip install -r requirements-gpu.txt; \
    fi

ENV PYTHONUNBUFFERED=1
ENV ONNX_AS_MODELS_DIR=/opt/wyoming-onnx-asr/data/models

EXPOSE 10300

ENTRYPOINT ["python3", "/opt/wyoming-onnx-asr/wyoming_onnx_asr.py"]
CMD ["--model", "parakeet-v3", "--uri", "tcp://0.0.0.0:10300"]
