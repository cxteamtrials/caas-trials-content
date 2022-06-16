FROM golang:alpine as builder

# Set necessary environmet variables needed for our image
ENV GO111MODULE=on \
    CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64

# Move to working directory /build
RUN mkdir -p /go/src/github.com/cxteamtrials/caas-trials-content/services/webapp
WORKDIR /go/src/github.com/cxteamtrials/caas-trials-content/services/webapp

# Copy and download dependency using go mod
COPY go.mod .
COPY go.sum .
RUN go mod download

# Copy the code into the container
COPY . .

# Build the application
RUN go build -o webapp .

FROM alpine:latest
RUN apk --no-cache add ca-certificates
RUN mkdir -p /go/src/github.com/cxteamtrials/caas-trials-content/services/webapp
WORKDIR /go/src/github.com/cxteamtrials/caas-trials-content/services/webapp
COPY --from=builder /go/src/github.com/cxteamtrials/caas-trials-content/services/webapp/webapp .
COPY --from=builder /go/src/github.com/cxteamtrials/caas-trials-content/services/webapp/static ./static
COPY --from=builder /go/src/github.com/cxteamtrials/caas-trials-content/services/webapp/views ./views
COPY --from=builder /go/src/github.com/cxteamtrials/caas-trials-content/services/webapp/conf ./conf
EXPOSE 8080
CMD ["./webapp"]
