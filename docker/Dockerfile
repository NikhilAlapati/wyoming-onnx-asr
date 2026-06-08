ARG BASE_IMAGE=nvidia/cuda:12.9.0-cudnn-runtime-ubuntu24.04
FROM ${BASE_IMAGE}

ARG CPU=0

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        python3 python3-venv python3-pip ca-certificates && \
    rm -rf /var/lib/apt/lists/* && \
    ln -sf /usr/bin/python3 /usr/local/bin/python

WORKDIR /opt/wyoming-onnx-asr
COPY . /opt/wyoming-onnx-asr

RUN python3 -m venv /opt/venv && \
    /opt/venv/bin/pip install --upgrade pip setuptools wheel && \
    if [ "${CPU}" = "1" ]; then \
        /opt/venv/bin/pip install -r requirements-cpu.txt; \
    else \
        /opt/venv/bin/pip install -r requirements-gpu.txt; \
    fi

ENV PATH="/opt/venv/bin:$PATH"

ENV PYTHONUNBUFFERED=1
ENV ONNX_AS_MODELS_DIR=/opt/wyoming-onnx-asr/data/models

EXPOSE 10300

ENTRYPOINT ["python3", "/opt/wyoming-onnx-asr/wyoming_onnx_asr.py"]
CMD ["--model", "parakeet-v3", "--uri", "tcp://0.0.0.0:10300"]
