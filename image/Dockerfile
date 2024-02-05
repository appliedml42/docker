FROM nvcr.io/nvidia/pytorch:23.10-py3

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y sudo wget unzip poppler-utils lsb-core software-properties-common

RUN useradd --uid 1000 --shell /bin/bash appliedml42
RUN usermod -aG sudo appliedml42
RUN echo "appliedml42 ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
ADD . /app
RUN chown -R appliedml42 /app
ENV HOME /app
WORKDIR /app
USER appliedml42
ENV PATH /app/.local/bin:$PATH

RUN pip install --trusted-host pypi.python.org -r ./requirements.txt

# Install SPM CLI Tools
RUN git clone https://github.com/google/sentencepiece.git
WORKDIR sentencepiece
RUN mkdir build
WORKDIR build
RUN cmake ..
RUN make -j $(nproc)
RUN sudo make install
RUN sudo ldconfig -v
WORKDIR /app

# Install Pytorch Nightly Last so it can overwrite the version installed by the base image
RUN pip3 install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cu121
