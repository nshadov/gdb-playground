FROM ubuntu:17.10

MAINTAINER nshadov

RUN apt-get update && apt-get install -y build-essential gdb gcc

WORKDIR /root
COPY code.c code.c
RUN gcc -g -gstabs+ code.c -o code

CMD ["/bin/bash"]
