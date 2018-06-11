# built from aws iot example makefile to create a library instead of an
# application creates a folder libs containing mbedtls libraries along with aws iot libraries

COMPILER_FLAGS += -Os
#COMPILER_FLAGS += -g

#IoT client directory

IOT_CLIENT_DIR = ..

PLATFORM_DIR = $(IOT_CLIENT_DIR)/platform/linux/mbedtls
PLATFORM_COMMON_DIR = $(IOT_CLIENT_DIR)/platform/linux/common

IOT_INCLUDE_DIRS += -I ./
IOT_INCLUDE_DIRS += -I $(IOT_CLIENT_DIR)/include
IOT_INCLUDE_DIRS += -I $(IOT_CLIENT_DIR)/external_libs/jsmn
IOT_INCLUDE_DIRS += -I $(PLATFORM_COMMON_DIR)
IOT_INCLUDE_DIRS += -I $(PLATFORM_DIR)
IOT_INCLUDE_DIRS += -I $(PLATFORM_DIR)/../pthread/

IOT_SRC_FILES += $(shell find $(IOT_CLIENT_DIR)/src/ -name '*.c')
IOT_SRC_FILES += $(shell find $(IOT_CLIENT_DIR)/external_libs/jsmn -name '*.c')
IOT_SRC_FILES += $(shell find $(PLATFORM_DIR)/ -name '*.c')
IOT_SRC_FILES += $(shell find $(PLATFORM_COMMON_DIR)/ -name '*.c')
IOT_SRC_FILES += $(shell find $(PLATFORM_DIR)/../pthread/ -name '*.c')

#TLS - mbedtls
export CC
export CFLAGS
MBEDTLS_DIR = $(IOT_CLIENT_DIR)/external_libs/mbedTLS
TLS_LIB_DIR = $(MBEDTLS_DIR)/library
TLS_INCLUDE_DIR = -I $(MBEDTLS_DIR)/include
EXTERNAL_LIBS += -L$(TLS_LIB_DIR)

#Aggregate all include and src directories
INCLUDE_ALL_DIRS += $(IOT_INCLUDE_DIRS)
INCLUDE_ALL_DIRS += $(TLS_INCLUDE_DIR)

SRC_FILES += $(IOT_SRC_FILES)

# Logging level control
#LOG_FLAGS += -DENABLE_IOT_INFO
#LOG_FLAGS += -DENABLE_IOT_DEBUG
#LOG_FLAGS += -DENABLE_IOT_WARN
#LOG_FLAGS += -DENABLE_IOT_ERROR
#LOG_FLAGS += -DENABLE_IOT_TRACE
#LOG_FLAGS += -DENABLE_IOT_BUFDUMP

COMPILER_FLAGS += $(LOG_FLAGS)

MBED_TLS_MAKE_CMD = $(MAKE) -C $(MBEDTLS_DIR) lib

PRE_MAKE_CMD = $(MBED_TLS_MAKE_CMD)

OBJS := $(IOT_SRC_FILES:.c=.o)

lib:$(OBJS)
	$(PRE_MAKE_CMD)
	$(AR) rcs libawsiot.a $^	
	mv $(TLS_LIB_DIR)/*.a lib/
	mv *.a lib/

clean:
	find $(IOT_CLIENT_DIR) -name "*.o" -type f -delete
	$(MAKE) -C $(MBEDTLS_DIR) clean
	rm -rf lib

%.o: %.c
	mkdir -p lib/
	$(CC) $(COMPILER_FLAGS) -c -o $@ $(INCLUDE_ALL_DIRS) $<
