#ARG BASE_TAG=main-base
ARG BASE_TAG=main-base-py3.11-cu121-2.2.1
FROM winglian/axolotl-base:$BASE_TAG

ARG TORCH_CUDA_ARCH_LIST="7.0 7.5 8.0 8.6+PTX"
ARG AXOLOTL_EXTRAS=""
ARG AXOLOTL_ARGS=""
#ARG CUDA="118"
ARG CUDA="121"
ENV BNB_CUDA_VERSION=$CUDA
#ARG PYTORCH_VERSION="2.1.2"
ARG PYTORCH_VERSION="2.2.1"

ENV PYTORCH_VERSION=$PYTORCH_VERSION

RUN apt-get update && \
    apt-get install -y --allow-change-held-packages vim curl nano libnccl2 libnccl-dev

WORKDIR /workspace

#RUN git clone --depth=1 https://github.com/OpenAccess-AI-Collective/axolotl.git
RUN git clone --depth=1 https://github.com/peterdonnelly1/axolotl axo

#WORKDIR /workspace/axolotl
WORKDIR /workspace/axo

# If AXOLOTL_EXTRAS is set, append it in brackets

RUN pip install xformers==0.0.21

RUN pip install causal_conv1d
RUN if [ "$AXOLOTL_EXTRAS" != "" ] ; then \
        pip install -e .[deepspeed,flash-attn,mamba-ssm,galore,$AXOLOTL_EXTRAS] $AXOLOTL_ARGS; \
    else \
        pip install -e .[deepspeed,flash-attn,mamba-ssm,galore] $AXOLOTL_ARGS; \
    fi

# So we can test the Docker image
RUN pip install pytest

# fix so that git fetch/pull from remote works
RUN git config remote.origin.fetch "+refs/heads/*:refs/remotes/origin/*" && \
    git config --get remote.origin.fetch

# helper for huggingface-login cli
RUN git config --global credential.helper store

RUN apt install tree
