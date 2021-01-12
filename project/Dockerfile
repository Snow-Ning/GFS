# Use pre-built image of ubuntu with bazel
# It's better to download a image on the local disk, then you can use it by loading from disk instead of net
FROM tugan0329/bazel-linux:latest
WORKDIR /app
RUN sed -i 's#http://archive.ubuntu.com/#http://mirrors.tuna.tsinghua.edu.cn/#' /etc/apt/sources.list;
RUN apt-get update
RUN apt-get install -y python python3
COPY WORKSPACE ./
COPY data ./data
COPY scripts ./scripts
COPY src/third_party ./src/third_party

#We copy the third party packages into containers
COPY src/manual_3 ./src/manual_3

# Build and cache protos generated code
COPY src/protos ./src/protos
RUN bazel build src/protos/...

# Build and cache common code
COPY src/common ./src/common
RUN bazel build src/common/...

# Build and cache client code
COPY src/client ./src/client
RUN bazel build src/client/...

# Build and cache server code
COPY src/server ./src/server
RUN bazel build src/server/...

# Build and cache rest of src code
COPY src/ ./src/
RUN bazel build src/...

# Build and cache rest of examaples
COPY examples/ ./examples
RUN bazel build examples/...

# Build and cache the rest of the code
COPY . .
RUN bazel build ...
