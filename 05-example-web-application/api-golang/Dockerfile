FROM golang:1.19-alpine AS build

WORKDIR /app

COPY ["go.mod", "go.sum", "./"]

RUN --mount=type=cache,target=/root/.cache/go-build \
  --mount=type=cache,target=/go/pkg/mod \
  go mod download

COPY . .

# https://mt165.co.uk/blog/static-link-go/
# RUN go build \
#   -ldflags="-linkmode external -extldflags -static" \
#   -tags netgo \
#   -o golang-api
# or https://docs.docker.com/guides/golang/build-images/#build-the-image
RUN CGO_ENABLED=0 GOOS=linux go build -o golang-api

USER nonroot

###
FROM scratch

ENV GIN_MODE=release

WORKDIR /app

COPY --from=build --chown=nonroot:nonroot /app/golang-api .

EXPOSE 8080

CMD ["./golang-api"]