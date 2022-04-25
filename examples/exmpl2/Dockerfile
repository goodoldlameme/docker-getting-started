FROM ubuntu
ENV AUTHOR="Anonymous"
COPY ["src", "src"]
WORKDIR /src
RUN apt-get update && apt-get install -y iputils-ping && apt-get install -y curl
ENTRYPOINT ["bash", "hello.sh"]
