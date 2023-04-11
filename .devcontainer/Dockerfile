FROM ubuntu
ENV DEBIAN_FRONTEND=noninteractive

RUN apt update -y && \
    apt install -y zsh && \
    adduser vscode

USER vscode

RUN echo 'source <(kubectl completion bash)' >> /home/vscode/.zshrc