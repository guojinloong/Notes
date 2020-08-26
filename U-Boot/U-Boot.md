# U-Boot
## U-Boot工程分析
### 工程文件和目录
### 顶层Makefile分析
#### 版本号
```shell
VERSION = 2016
PATCHLEVEL = 03
SUBLEVEL =
EXTRAVERSION =
NAME =
```
#### MAKEFLAGS变量
  顶层Makefile在调用子目录中的Makefile编译时，需要通过"export"向子make传递环境变量，使用"unexport"取消传递。有两个特殊的环境变量："SHELL"和"MAKESFLAG"，在整个make过程中自动传递给子make。
```shell
MAKEFLAGS += -rR --include-dir=$(CURDIR)
```
  使用"+="追加参数，"-rR"禁止使用内置的隐含规则和变量定义，"--include-dir"指明搜索路径，"$(CURDIR)"表示当前目录。
#### 命令输出
```shell
ifeq ("$(origin V)", "command line")
  KBUILD_VERBOSE = $(V)
endif
ifndef KBUILD_VERBOSE
  KBUILD_VERBOSE = 0
endif

ifeq ($(KBUILD_VERBOSE),1)
  quiet =
  Q =
else
  quiet=quiet_
  Q = @
endif

...

ifneq ($(filter 4.%,$(MAKE_VERSION)),)	# make-4
ifneq ($(filter %s ,$(firstword x$(MAKEFLAGS))),)
  quiet=silent_
endif
else					# make-3.8x
ifneq ($(filter s% -s%,$(MAKEFLAGS)),)
  quiet=silent_
endif
endif

export quiet Q KBUILD_VERBOSE
```
  使用origin函数判断变量V是否来源于命令行，然后设置变量"quiet”和”Q"的值，用来控制编译时是否在终端输出完整的命令。
|变量名|值|作用|
|-|-|-|
|quiet|quiet\_、silent\_|有些命令会有2个版本，如"quiet\_\<cmd>"，带"quiet\_"前缀的命令执行时输出的信息少即短命令，否则输出完整的命令即长命令。当前缀为"silent\_"时什么都不输出，因为该命令不存在。|
|Q|@|在make命令前加上"@"后就不会在终端输出命令了。|
  若V=1，将"quiet”和"Q"设为空，编译时显示完整的命令；若V=0或没有定义，设置"quiet=quiet\_和Q=@"，编译时显示短命令。
  使用"make -s"编译时不输出任何信息，即静默模式。
#### 设置输出目录
  在编译时使用"make O=xxx"命令设置目标文件输出目录，可以将源文件和编译生成的文件分开，否则默认目标文件与对应源文件在相同目录。
```shell
# kbuild supports saving output files in a separate directory.
# To locate output files in a separate directory two syntaxes are supported.
# In both cases the working directory must be the root of the kernel src.
# 1) O=
# Use "make O=dir/to/store/output/files/"
#
# 2) Set KBUILD_OUTPUT
# Set the environment variable KBUILD_OUTPUT to point to the directory
# where the output files shall be placed.
# export KBUILD_OUTPUT=dir/to/store/output/files/
# make
#
# The O= assignment takes precedence over the KBUILD_OUTPUT environment
# variable.

# KBUILD_SRC is set on invocation of make in OBJ directory
# KBUILD_SRC is not intended to be used by the regular user (for now)
ifeq ($(KBUILD_SRC),)

# OK, Make called in directory where kernel src resides
# Do we want to locate output files in a separate directory?
ifeq ("$(origin O)", "command line")
  KBUILD_OUTPUT := $(O)
endif

# That's our default target when none is given on the command line
PHONY := _all
_all:

# Cancel implicit rules on top Makefile
$(CURDIR)/Makefile Makefile: ;

ifneq ($(KBUILD_OUTPUT),)
# Invoke a second make in the output directory, passing relevant variables
# check that the output directory actually exists
saved-output := $(KBUILD_OUTPUT)
KBUILD_OUTPUT := $(shell mkdir -p $(KBUILD_OUTPUT) && cd $(KBUILD_OUTPUT) \
								&& /bin/pwd)
$(if $(KBUILD_OUTPUT),, \
     $(error failed to create output directory "$(saved-output)"))

PHONY += $(MAKECMDGOALS) sub-make

$(filter-out _all sub-make $(CURDIR)/Makefile, $(MAKECMDGOALS)) _all: sub-make
	@:

sub-make: FORCE
	$(Q)$(MAKE) -C $(KBUILD_OUTPUT) KBUILD_SRC=$(CURDIR) \
	-f $(CURDIR)/Makefile $(filter-out _all sub-make,$(MAKECMDGOALS))

# Leave processing to above invocation of make
skip-makefile := 1
endif # ifneq ($(KBUILD_OUTPUT),)
endif # ifeq ($(KBUILD_SRC),)

# We process the rest of the Makefile if this is the final invocation of make
ifeq ($(skip-makefile),)

# Do not print "Entering directory ...",
# but we want to display it when entering to the output directory
# so that IDEs/editors are able to understand relative filenames.
MAKEFLAGS += --no-print-directory
```
#### 代码检查
  U-Boot在编译时可以使能代码检查，使用"make C=1"检查需要重新编译的文件，使用"make C=2"检查所有源代码，C没定义或=0则不检查。
```shell
# Call a source code checker (by default, "sparse") as part of the
# C compilation.
#
# Use 'make C=1' to enable checking of only re-compiled files.
# Use 'make C=2' to enable checking of *all* source files, regardless
# of whether they are re-compiled or not.
#
# See the file "Documentation/sparse.txt" for more details, including
# where to get the "sparse" utility.

ifeq ("$(origin C)", "command line")
  KBUILD_CHECKSRC = $(C)
endif
ifndef KBUILD_CHECKSRC
  KBUILD_CHECKSRC = 0
endif
```

#### 模块编译
  U-Boot编译时可以使用"make M=dir"（新）或"make SUBDIRS=dir"（旧）单独编译某个模块，前一个优先级更高。
```shell
# Use make M=dir to specify directory of external module to build
# Old syntax make ... SUBDIRS=$PWD is still supported
# Setting the environment variable KBUILD_EXTMOD take precedence
ifdef SUBDIRS
  KBUILD_EXTMOD ?= $(SUBDIRS)
endif

ifeq ("$(origin M)", "command line")
  KBUILD_EXTMOD := $(M)
endif

# If building an external module we do not care about the all: rule
# but instead _all depend on modules
PHONY += all
ifeq ($(KBUILD_EXTMOD),)
_all: all
else
_all: modules
endif

ifeq ($(KBUILD_SRC),)
        # building in the source tree
        srctree := .
else
        ifeq ($(KBUILD_SRC)/,$(dir $(CURDIR)))
                # building in a subdirectory of the source tree
                srctree := ..
        else
                srctree := $(KBUILD_SRC)
        endif
endif
objtree		:= .
src		:= $(srctree)
obj		:= $(objtree)

VPATH		:= $(srctree)$(if $(KBUILD_EXTMOD),:$(KBUILD_EXTMOD))

export srctree objtree VPATH

# Make sure CDPATH settings don't interfere
unexport CDPATH
```
#### 获取主机架构和系统
  定义变量HOSTARCH，使用"uname -m"获取当前主机架构，使用"sed -e s/i.86/x86/"将"i.86"替换成"x86"......
  定义变量HOSTOS，使用"uname -s"获取当前主机系统，使用"tr '[:upper:]' '[:lower:]'"将大写字母替换为小写，使用"sed -e 's/\(cygwin\).*/sygwin/'"将"cygwin.\*"替换为"cygwin"。
```shell
HOSTARCH := $(shell uname -m | \
	sed -e s/i.86/x86/ \
	    -e s/sun4u/sparc64/ \
	    -e s/arm.*/arm/ \
	    -e s/sa110/arm/ \
	    -e s/ppc64/powerpc/ \
	    -e s/ppc/powerpc/ \
	    -e s/macppc/powerpc/\
	    -e s/sh.*/sh/)

HOSTOS := $(shell uname -s | tr '[:upper:]' '[:lower:]' | \
	    sed -e 's/\(cygwin\).*/cygwin/')

export	HOSTARCH HOSTOS
```
#### 定义目标架构、交叉编译器和配置文件
  编译时使用"make ARCH=arm CROSS_COMPILE=arm-linux-gnu-eabihf-"制定目标架构和交叉编译器，也可以在顶层Makefile中指定ARCH和CROSS_COMPILE。
  "configs"目录下存储了U-Boot的初始配置文件，使用"make xxx_defconfig"后将初始配置文件复制并重命名为".config"，之后使用"make menuconfig"等命令修改配置后都保存在".config"。使用"make distclean"会删除该配置文件，需要重新配置，使用"make clean"不会。
```shell
# set default to nothing for native builds
ifeq ($(HOSTARCH),$(ARCH))
CROSS_COMPILE ?=
endif

# Set default value for ARCH and CROSS_COMPILE
ARCH ?= arm
CROSS_COMPILE ?= arm-linux-gnueabihf-

KCONFIG_CONFIG	?= .config
export KCONFIG_CONFIG

...

```
#### 调用scripts/Kbuild.include
```shell
# We need some generic definitions (do not try to remake the file).
scripts/Kbuild.include: ;
include scripts/Kbuild.include
```
#### 交叉编译工具变量设置

## U-Boot编译过程
## U-Boot启动流程
## U-Boot移植
## U-Boot图形化配置