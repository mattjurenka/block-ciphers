# Build Stage
FROM --platform=linux/amd64 rustlang/rust:nightly as builder

ENV DEBIAN_FRONTEND=noninteractive
## Install build dependencies.
RUN apt-get update 
RUN apt-get install -y cmake clang
RUN cargo install cargo-fuzz

## Add source code to the build stage.
ADD . /block-ciphers/

WORKDIR /block-ciphers/fuzz/

RUN cargo +nightly fuzz build

FROM --platform=linux/amd64 rustlang/rust:nightly

## TODO: Change <Path in Builder Stage>

COPY --from=builder /block-ciphers/fuzz/target/x86_64-unknown-linux-gnu/release/aes_encrypt /


COPY --from=builder /block-ciphers/fuzz/target/x86_64-unknown-linux-gnu/release/aes192_encrypt /

COPY --from=builder /block-ciphers/fuzz/target/x86_64-unknown-linux-gnu/release/aes256_encrypt /

COPY --from=builder /block-ciphers/fuzz/target/x86_64-unknown-linux-gnu/release/threefish_256 /

COPY --from=builder /block-ciphers/fuzz/target/x86_64-unknown-linux-gnu/release/threefish_512 /

COPY --from=builder /block-ciphers/fuzz/target/x86_64-unknown-linux-gnu/release/threefish_1024 /

COPY --from=builder /block-ciphers/fuzz/target/x86_64-unknown-linux-gnu/release/twofish /