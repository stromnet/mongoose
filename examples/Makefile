# Copyright (c) 2014 Cesanta Software
# All rights reserved
#
# Makefile for Mongoose web server examples

CFLAGS = -W -Wall -I.. -pthread -g -pipe $(CFLAGS_EXTRA)
RM = rm -rf
OUT = -o $@
ALL_PROGS = hello websocket server post multi_threaded upload auth pubsub
NS = ../../net_skeleton
SW = ../../ssl_wrapper

ifeq ($(OS),Windows_NT)
  ifndef MINGW
    MSVC = ../../vc6
    RM = del /q /f
    OUT =
    CC = $(MSVC)/bin/cl
    CFLAGS = /MD /TC /nologo /W3 /I$(MSVC)/include /I..
    CFLAGS += /link /incremental:no /libpath:$(MSVC)/lib /machine:IX86
    CFLAGS += $(CFLAGS_EXTRA)
  else
    CC = g++
    CFLAGS = -W -Wall -Wno-unused-parameter -I.. -O0 -g -pipe $(CFLAGS_EXTRA) -lwsock32
  endif
else
  UNAME_S := $(shell uname -s)
  CC = g++
  CFLAGS += -lssl -g -O0

  ifeq ($(UNAME_S),Linux)
    CFLAGS += -ldl
  endif

  ifeq ($(UNAME_S),Darwin)
  endif
endif

all: $(ALL_PROGS)

# To enable Lua in a server, uncomment following lines
LUA    = ../lua-5.2.3/src
#CFLAGS += -I$(LUA) -L$(LUA) -llua -DMONGOOSE_USE_LUA

server: server.c ../mongoose.c
	$(CC) server.c ../mongoose.c $(OUT)  $(CFLAGS)

hello: hello.c ../mongoose.c
	$(CC) hello.c ../mongoose.c $(OUT) $(CFLAGS)

websocket: websocket_html.c websocket.c ../mongoose.c
	$(CC) websocket.c websocket_html.c ../mongoose.c $(OUT)  $(CFLAGS)

post: post.c ../mongoose.c
	$(CC) post.c ../mongoose.c $(OUT) $(CFLAGS)

multi_threaded: multi_threaded.c ../mongoose.c
	$(CC) multi_threaded.c ../mongoose.c $(OUT) $(CFLAGS)

upload: upload.c ../mongoose.c
	$(CC) upload.c ../mongoose.c $(OUT) $(CFLAGS)

auth: auth.c ../mongoose.c
	$(CC) auth.c ../mongoose.c $(OUT) $(CFLAGS)

file: file.c ../mongoose.c
	$(CC) file.c ../mongoose.c $(OUT) $(CFLAGS)

form: form.c ../mongoose.c
	$(CC) form.c ../mongoose.c $(OUT) $(CFLAGS)

mjpg: mjpg.c ../mongoose.c
	$(CC) mjpg.c ../mongoose.c $(OUT) $(CFLAGS)

pubsub: websocket2.c ../mongoose.c
	$(CC) websocket2.c ../mongoose.c $(OUT) $(CFLAGS)

proxy: proxy.c ../mongoose.c
	$(CC) proxy.c ../mongoose.c $(OUT) -I$(NS) -DNS_ENABLE_SSL -lssl $(CFLAGS)

websocket_html.c: websocket.html
	perl mkdata.pl $< > $@

u:
	$(CC) unit_test.c $(OUT) $(CFLAGS)
	./$@

clean:
	-@$(RM) $(ALL_PROGS) websocket_html.c *.exe *.dSYM *.obj *.exp .*o *.lib u
