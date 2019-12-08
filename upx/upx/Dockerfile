FROM alpine:3.10.3 as build_upx
RUN apk --no-cache add git

ENV UCL_VERSION=1.03
ENV UPX_UCLDIR=/ucl-$UCL_VERSION
RUN wget -O  /ucl.tar.gz http://www.oberhumer.com/opensource/ucl/download/ucl-$UCL_VERSION.tar.gz
RUN tar -xvzf /ucl.tar.gz
WORKDIR /$UPX_UCLDIR

# There is a package upx under alpine already

RUN apk --no-cache add make musl-dev binutils clang clang-dev g++
ENV CXX="clang++"
ENV CC="clang"

RUN ./configure
RUN make

WORKDIR /
ENV UPX_VERSION=tags/v3.95
RUN git clone https://github.com/upx/upx
WORKDIR /upx
RUN git checkout $UPX_VERSION
RUN git submodule update --init --recursive

RUN sed 's/^#elif (ACC_ARCH_AMD64 || ACC_ARCH_IA64)$/& \&\& !defined(__ILP32__)/' $UPX_UCLDIR/acc/acc_chk.ch > a.tmp
RUN if cmp -s a.tmp $UPX_UCLDIR/acc/acc_chk.ch; then rm a.tmp; else mv a.tmp $UPX_UCLDIR/acc/acc_chk.ch; fi

# bash==trash
RUN apk --no-cache add zlib-dev bash perl-dev

RUN make all -j
RUN /upx/src/upx.out /upx/src/upx.out

FROM alpine:3.10.3
RUN apk --no-cache add libgcc libstdc++
COPY --from=build_upx /upx/src/upx.out /upx
ENTRYPOINT ["/upx"]