## https://hub.docker.com/_/rust
## https://github.com/rust-lang/docker-rust/blob/master/1.71.0/bookworm/slim/Dockerfile
## docker build -t my_debian_bookworm_node_fnm .
## docker container rm -f debian_bookworm_node_fnm_container && docker run -it --name debian_bookworm_node_fnm_container my_debian_bookworm_node_fnm
FROM rust:slim-bookworm

RUN mkdir /root/.fnm

RUN apt-get update && apt-get install -y curl unzip

## RUN curl -fsSL https://fnm.vercel.app/install | bash ## this line throw error: Can't infer shell!
## (fix error) -> https://github.com/Schniz/fnm/issues/412
RUN curl -fsSL https://fnm.vercel.app/install | bash -s -- --skip-shell

RUN echo '# fnm' >> /root/.bashrc
RUN echo 'export PATH=/root/.fnm:$PATH' >> /root/.bashrc
RUN echo 'eval "$(fnm env --shell bash)"' >> /root/.bashrc

CMD ["bash"]