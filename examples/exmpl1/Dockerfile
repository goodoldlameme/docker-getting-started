FROM ubuntu
ENV AUTHOR="Anonymous"
COPY ["src", "src"]
WORKDIR /src
RUN apt-get update && apt-get install -y iputils-ping
ENTRYPOINT ["bash", "hello.sh"]
