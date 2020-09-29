#Build Libfake
FROM gcc as libfaketime
WORKDIR /
RUN git clone https://github.com/wolfcw/libfaketime.git \
    && cd libfaketime \
    && make install \
    && wget https://curl.haxx.se/download/curl-7.52.1.tar.gz \
    && tar -xvf curl-7.52.1.tar.gz \
    && cd curl-7.52.1 \
    && ./configure \
    && make \
    && make install

#Run app with faked time
FROM adoptopenjdk/openjdk8:ubi-minimal-jre
RUN mkdir /opt/app
COPY /target/*-exec.jar /opt/app/optaweb-employee-rostering-standalone.jar
COPY --from=libfaketime /usr/local/lib/faketime/libfaketime.so.* /usr/local/lib/faketime/libfaketime
CMD ["/bin/bash", "-c", "LD_PRELOAD=/usr/local/lib/faketime/libfaketime FAKETIME_DONT_RESET=1 FAKETIME_NO_CACHE=1 FAKETIME=\"@2017-12-01 00:00:00\" java -jar /opt/app/optaweb-employee-rostering-standalone.jar"]
EXPOSE 8080


