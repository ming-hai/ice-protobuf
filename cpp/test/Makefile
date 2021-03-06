# **********************************************************************
#
# Copyright (c) 2003-2017 ZeroC, Inc. All rights reserved.
#
# **********************************************************************

top_srcdir	= ..

CLIENT		= client
SERVER		= server

TARGETS		= $(CLIENT) $(SERVER)

COBJS		= Test.o \
			Client.o \
			AllTests.o \
			Test.pb.o

SOBJS		= Test.o \
			TestI.o \
			Server.o \
			Test.pb.o

SRCS		= $(COBJS:.o=.cpp) \
			$(SOBJS:.o=.cpp)

SLICE_SRCS	= Test.ice
PROTO_SRCS	= Test.proto

include $(top_srcdir)/make/Make.rules

CPPFLAGS	:= $(ICE_FLAGS) -I. -I$(PROTOBUF_HOME)/include $(CPPFLAGS)
LIBS		:= $(ICE_LIB_DIR) $(LIBS) -L$(PROTOBUF_HOME)/lib -lprotobuf

$(CLIENT): $(COBJS)
	rm -f $@
	$(CXX) $(LDFLAGS) $(LDEXEFLAGS) -o $@ $(COBJS) $(LIBS)

$(SERVER): $(SOBJS)
	rm -f $@
	$(CXX) $(LDFLAGS) $(LDEXEFLAGS) -o $@ $(SOBJS) $(LIBS)

%..pb.h %.pb.cpp: %.proto
	rm -f $(*F).pb.h $(*F).pb.cpp
	$(PROTOBUF_HOME)/bin/protoc --cpp_out=. $(*F).proto
	mv $(*F).pb.cc $(*F).pb.cpp

depend::
	for i in $(PROTO_SRCS); do \
		echo "$${i%.proto}.pb.cpp: $$i" >> .depend; \
	done

clean::
	rm -f $(addsuffix .pb.cpp, $(basename $(notdir $(PROTO_SRCS))))
	rm -f $(addsuffix .pb.h, $(basename $(notdir $(PROTO_SRCS))))
