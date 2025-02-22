FROM ubuntu:20.04
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    curl \
    gcc libc6-dev ca-certificates

ENV RUSTUP_INIT_SKIP_PATH_CHECK="yes"
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y -v --no-modify-path
RUN cp ${HOME}/.cargo/bin/* /usr/local/bin/

RUN cargo install --git https://github.com/solana-labs/cargo-run-sbf-tests.git \
    --rev 8b76e26ee3cddca60297847361b67603e4299209 \
    --bin cargo-run-sbf-tests --root /usr/local

RUN mkdir -p /tmp/.cache/solana/v1.29/sbf-tools
RUN curl -L -o solana-sbf-tools-linux.tar.bz2 https://github.com/solana-labs/sbf-tools/releases/download/v1.29/solana-sbf-tools-linux.tar.bz2
RUN tar -xjf solana-sbf-tools-linux.tar.bz2 --strip-components 1 -C /tmp/.cache/solana/v1.29/sbf-tools
RUN rustup toolchain link sbf /tmp/.cache/solana/v1.29/sbf-tools/rust
RUN cp -R ${HOME}/.rustup /tmp/

ENV CARGO_TARGET_SBF_SOLANA_SOLANA_RUNNER="cargo-run-sbf-tests --heap-size 104857600"
ENV CC="/tmp/.cache/solana/v1.29/sbf-tools/llvm/bin/clang"
ENV RUSTUP_TOOLCHAIN="sbf"
