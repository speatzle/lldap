FROM rust:1.66

ARG USERNAME=lldapdev
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

RUN apt update && \
    apt install -y --no-install-recommends libssl-dev musl-dev make perl curl gzip && \
    rm -rf /var/lib/apt/lists/*

RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install wasm-pack \
    && rustup target add wasm32-unknown-unknown

USER $USERNAME
ENV CARGO_HOME=/home/$USERNAME/.cargo
ENV SHELL=/bin/bash