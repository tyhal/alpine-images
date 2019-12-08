FROM alpine:3.10.3

RUN apk --no-cache add git
ENV GODOT_VERSION=tags/3.1.1-stable
RUN git clone https://github.com/godotengine/godot.git
WORKDIR /godot
RUN git checkout $GODOT_VERSION

RUN apk --no-cache add scons musl-dev binutils g++
ENV CXX=clang++
ENV CC=clang

# clang clang-dev
#    use_llvm=yes \
# Cannot find <new>

RUN scons \
    platform=server \
    tools=no \
    target=release \
    bits=64 \
    -j $(nproc)
