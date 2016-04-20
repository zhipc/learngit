# 指定.h文件路径
INCDIR = ../include
# 指定编译器
CC = gcc 
# 将INCDIR目录作为第一个寻找头文件的目录
CFLAGS = -I$(INCDIR)

# 指定.o文件存储路径
OBJDIR = obj 
# 指定库文件路径
LIBDIR = ../lib

# 指定链接库
LIBS = -lm 

_DEPS = main.h
# $(patsubst %.c,%.o,$(dir)): 将dir目录下的所有.c文件替换为.o文件（patsubst为替换通配符）
DEPS = $(patsubst %, $(INCDIR)/%, $(_DEPS))
#DEPS = $(INCDIR)/%.h

_OBJ = main.o fun.o
OBJ = $(patsubst %, $(OBJDIR)/%, $(_OBJ))
#OBJ = $(OBJDIR)/%.o

# hellomake这个主目标一定要放到次目标$(OBJDIR)/%.o之前，否则make不通过
hellomake: $(OBJ)
# $^: 所有的依赖目标集合
    $(CC) $^ $(CFLAGS) -Wall -o $@
#$(OBJ): %.c $(DEPS)
$(OBJDIR)/%.o: %.c $(DEPS)
# $@: %.o(:的左面)
# $<: %.c(依赖关系的第一项)
    $(CC) -c $< $(CFLAGS) -o $@

test:
    @echo "obj is $(OBJ)"
    @echo "deps is $(DEPS)"
    @echo "obj is $($(OBJDIR)/%.o)"

install:
    ./hellomake

.PHONY: clean
clean:
# "%":Makefile中的通配符，但在函数调用时用*
    rm -rf hellomake *.o $(OBJDIR)/*.o
