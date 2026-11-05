FROM golang:1.26-alpine AS builder

WORKDIR /src

COPY go.mod ./
RUN go mod download

COPY cmd/ ./cmd/

ARG VERSION=dev
ARG COMMIT=unknown

RUN CGO_ENABLED=0 GOOS=linux go build \
    -trimpath \
    -ldflags="-s -w -X main.version=${VERSION} -X main.commit=${COMMIT}" \
    -o /out/server \
    ./cmd/server


FROM scratch

COPY --from=builder /out/server /server

USER 65532:65532

EXPOSE 8080

ENTRYPOINT ["/server"]