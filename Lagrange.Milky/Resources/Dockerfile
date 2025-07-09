FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:9.0-alpine AS build

WORKDIR /root/build
ARG TARGETARCH

RUN apk add --upgrade --no-cache \
        build-base \
        clang \
        zlib-dev
COPY --link . /root/build

RUN dotnet publish Lagrange.Milky -p:DebugType="none" -c Release -a $TARGETARCH -o /root/out

# ===

FROM alpine

WORKDIR /app/data

COPY --link --from=tianon/gosu /gosu /usr/local/bin/
COPY --link Lagrange.Milky/Resources/docker-entrypoint.sh /app/bin/docker-entrypoint.sh

COPY --link --from=build /root/out /app/bin

ENTRYPOINT ["/app/bin/docker-entrypoint.sh"]
