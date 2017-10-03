TARGET  := readhistory.exe
INCS = -I ../network -I ../utils
LIBS = -L ../utils -lutils -L ../network -lnetwork -Wl,-Bstatic -lev -ljansson -lmpdec -lmysqlclient -lz -lhiredis -Wl,-Bdynamic -lm -lpthread -ldl
# libmysqlclient20 seems to link in libstdc++
LIBS += -lstdc++
include ../makefile.inc
