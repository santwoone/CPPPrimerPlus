#1、读入所有的Makefile。
#2、读入被include的其它Makefile。
#3、初始化文件中的变量。
#4、推导隐晦规则，并分析所有规则。
#5、为所有的目标文件创建依赖关系链。
#6、根据依赖关系，决定哪些目标要重新生成。
#7、执行生成命令。

EXECUTABLE := myfirst    # 可执行文件名
#LIBDIR:=              # 静态库目录
#LIBS :=               # 静态库文件名
INCLUDES:=.           # 头文件目录
#SRCDIR:=              # 除了当前目录外，其他的源代码文件目录
CC:=g++
CFLAGS := -g -Wall -O3
#CFLAGS 表示用于 C 编译器的选项，
#CXXFLAGS 表示用于 C++ 编译器的选项。
CPPFLAGS := $(CFLAGS)
CPPFLAGS += $(addprefix -I,$(INCLUDES))
#addprefix函数：2个参数。将源串（第2个参数，由空格分隔）中的每一项添加前缀（第1个参数
#如果要引用其它目录下的头文件，其gcc命令如下，采用-I的参数： gcc -I /ycl/include/ helloworld.c -o helloworld
#如果要引用其它的目录下的库文件，如libmlib.so，其gcc命令如下，采用-L的参数： gcc -L /ycl/lib –lmlib helloworld.c -o helloworld
CPPFLAGS += -MMD
# -MMD 可以用于自动生成头文件依赖关系
#addprefix函数：2个参数。将源串（第2个参数，由空格分隔）中的每一项添加前缀（第1个参数）
RM-F := rm -f
SRCS := myfirst.cpp
#SRCS := $(wildcard *.cpp) $(wildcard $(addsuffix /*.cpp, $(SRCDIR)))

#SOURCES = $(wildcard *.cpp) 列出工作目录下文件名满足"*.cpp"条件的文件，以空格分隔，并将列表赋给SOURCE变量
OBJS := $(patsubst %.cpp,%.o,$(SRCS))
DEPS := $(patsubst %.o,%.d,$(OBJS))
MISSING_DEPS := $(filter-out $(wildcard $(DEPS)),$(DEPS))
#实现了去除变量$(DEPS)中$(wildcard $(DEPS))定义的字串（文件名）功能。
MISSING_DEPS_SOURCES := $(wildcard $(patsubst %.d,%.cpp,$(MISSING_DEPS)))

.PHONY : all deps objs clean veryclean rebuild 

all: $(EXECUTABLE)
#all : 作为第一个出现的目标项目，Make会将它作为主要和缺省项目("make"就表示"make all")
deps : $(DEPS)
#deps : 只生成依赖文件(.d文件)
objs : $(OBJS)
#objs : 为每一个源码程序生成或更新 '.d' 文件和'.o'文件
clean :
	        @$(RM-F) *.o
	        @$(RM-F) *.d
	        @$(RM-F) $(EXECUTABLE)

rebuild: all
	ifneq ($(MISSING_DEPS),)
$(MISSING_DEPS) :
	        @$(RM-F) $(patsubst %.d,%.o,$@)
		endif
		-include $(DEPS)
$(EXECUTABLE) : $(OBJS)
	        $(CC) -o $(EXECUTABLE) $(OBJS) $(addprefix -L,$(LIBDIR)) $(addprefix -l,$(LIBS))

