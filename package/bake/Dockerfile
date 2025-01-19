ARG VERSION=2.4.1-2
FROM docker.opsbox.dev/seanly/toolset:openjdk-8u372-b07-2 AS build

WORKDIR /code/dm8
COPY ./ ./

RUN <<EOF
cp -r /maven-wrapper/mvnw ./
cp -r /maven-wrapper/.mvn ./

./mvnw install:install-file -Dfile=./libs/DmJdbcDriver18-8.1.3.140.jar \
    -DgroupId=com.dameng -DartifactId=DmJdbcDriver18 -Dversion=8.1.3.140 \
    -Dpackaging=jar

./mvnw -B -e -U -DskipTests clean install
cp -r target/*.jar libs/
EOF

FROM docker.opsbox.dev/seanly/appset:nacos-${VERSION}

LABEL maintainer="seanly <seanly@opsbox.dev>"
COPY --from=build /code/dm8/libs/ /home/nacos/plugins/

EXPOSE 8848

ENTRYPOINT ["bash", "bin/start.sh"]