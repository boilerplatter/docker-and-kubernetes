FROM gcc AS builder
COPY hello.c /
RUN gcc -static hello.c

FROM scratch AS server
COPY --from=builder a.out /
CMD ["/a.out"]
