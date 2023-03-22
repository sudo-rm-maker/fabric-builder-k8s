# SPDX-License-Identifier: Apache-2.0

ARG GO_VER=1.17.5
ARG ALPINE_VER=3.14

FROM golang:${GO_VER}-alpine${ALPINE_VER} as build

ADD . $GOPATH/go-contract
WORKDIR $GOPATH/go-contract

ENV GOPROXY=https://goproxy.cn,direct
RUN go build -o go-contract .

FROM golang:${GO_VER}-alpine${ALPINE_VER}

LABEL org.opencontainers.image.title "Sample Go ERC-20 Contract"
LABEL org.opencontainers.image.description "Sample Hyperledger Fabric Go ERC-20 contract for Kubernetes chaincode builder"

COPY --from=build /usr/bin/dumb-init /usr/bin/dumb-init
COPY --from=build /go/bin/go-contract /usr/bin/go-contract

WORKDIR /var/hyperledger/go-contract
ENTRYPOINT ["/usr/bin/dumb-init", "--"]
CMD ["sh", "-c", "exec /usr/bin/go-contract -peer.address=$CORE_PEER_ADDRESS"]