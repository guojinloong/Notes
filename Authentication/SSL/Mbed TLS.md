简介
===
  [Mbed TLS](https://tls.mbed.org/)实现了密码学原语、X.509认证和SSL/TLS及DTLS协议，使用C语言编写，代码占用空间小，适合嵌入式系统，是替代OpenSSL的一个不错的选择。

文件
------
|文件和路径名|说明|
|---|---|
|configs/|提供了一些针对某些具体应用的非标准的配置文件以供参考，详情查看configs/README.txt。|
|include/|<br>需要包含的头文件</br><br>mbedtls/mbedtls_config.h：配置平台依赖项和功能特性</br><br>mbedtls/build_info.h：定义Mbed TLS版本信息、包含配置文件（mbedtls/mbedtls_config.h或使用MBEDTLS_CONFIG_FILE指定）并检查（在文件最后包含了mbedtls/check_confg.h，如果开启了某些功能，但是没有开启它的依赖项，编译器会直接报错）。</br>|
|library/|源代码|
|programs/|包含了许多不同功能和用途的示例程序，用来演示库的特定功能。|
|tests/|使用Python根据tests/suites/下的\*.fuction和\*.data文件生成测试文件\*.c。function文件包含测试函数，data文件包含测试用例，指定将传递的测试函数的参数。|

移植&配置
------
  Mbed TLS可跨不同的架构和运行时环境进行移植，并且可以在各种不同的操作系统和裸机执行。只需要替换平台特定代码，而不用修改核心代码。
  可以手动修改include/mbedtls/mbedtls_config.h或者使用scripts/config.py（--help）脚本配置平台依赖项和功能特性。另外，可以在IDE中添加宏MBEDTLS_CONFIG_FILE指定config文件替换include/mbedtls/mbedtls_config.h，参考include/mbedtls/build_info.h。

* Networking
  网络模块net_sockets.c适用于实现socket API的Windows和Unix系统，SSL/TLS模块通过回调函数使用它。
  可以在include/mbedtls/mbedtls_config.h中禁用，并通过mbedtls_ssl_set_bio()替换为自己编写的阻塞或非阻塞读写函数，并设置超时时间。
```C
//#define MBEDTLS_NET_C
```

* Timing
  时间模块timing.c适用于Windows、Linux和BSD（包括OS X），SSL/TLS模块可选择性地通过DTLS的回调函数使用它，如果不使用DTLS则不需要时间函数。
  可以在include/mbedtls/mbedtls_config.h中禁用，并通过mbedtls_ssl_set_timer_cb()替换为自己编写的时序函数。
```C
//#define MBEDTLS_TIMING_C
```

* Default entropy source
  熵池是RNG模块的一部分，收集并安全地混合来自各种源的熵。在提供/dev/urandom的Windows和不同的Unix平台上，注册了一个默认的基于操作系统的源。
  可以编写一个或多个熵收集函数替换此源，在运行时通过mbedtls_entropy_add_source()函数注册。如果是基于硬件源，则在include/mbedtls/mbedtls_config.h中使能。
```C
#define MBEDTLS_ENTROPY_HARDWARE_ALT
```

* Hardware acceleration
  可以将大多数模块的加密原语实现替换为自定义的实现，便于使用相应的硬件加速。
  可以在include/mbedtls/mbedtls_config.h中使能MBEDTLS_MODULE_NAME_ALT替换整个模块，也可以使能MBEDTLS_FUNCTION_NAME_ALT仅替换一个功能。
```C
#define MBEDTLS_AES_ALT  //替换整个AES模块
//#define MBEDTLS_ARIA_ALT
//#define MBEDTLS_CAMELLIA_ALT
//#define MBEDTLS_CCM_ALT
//#define MBEDTLS_CHACHA20_ALT
//#define MBEDTLS_CHACHAPOLY_ALT
//#define MBEDTLS_CMAC_ALT
//#define MBEDTLS_DES_ALT
//#define MBEDTLS_DHM_ALT
//#define MBEDTLS_ECJPAKE_ALT
//#define MBEDTLS_GCM_ALT
//#define MBEDTLS_NIST_KW_ALT
//#define MBEDTLS_MD5_ALT
//#define MBEDTLS_POLY1305_ALT
//#define MBEDTLS_RIPEMD160_ALT
//#define MBEDTLS_RSA_ALT
//#define MBEDTLS_SHA1_ALT
//#define MBEDTLS_SHA256_ALT
//#define MBEDTLS_SHA512_ALT
```

```C
//#define MBEDTLS_MD5_PROCESS_ALT
//#define MBEDTLS_RIPEMD160_PROCESS_ALT
//#define MBEDTLS_SHA1_PROCESS_ALT
//#define MBEDTLS_SHA256_PROCESS_ALT
//#define MBEDTLS_SHA512_PROCESS_ALT
//#define MBEDTLS_DES_SETKEY_ALT
//#define MBEDTLS_DES_CRYPT_ECB_ALT
//#define MBEDTLS_DES3_CRYPT_ECB_ALT
//#define MBEDTLS_AES_SETKEY_ENC_ALT
//#define MBEDTLS_AES_SETKEY_DEC_ALT
#define MBEDTLS_AES_ENCRYPT_ALT  //仅替换AES模块的加密功能
//#define MBEDTLS_AES_DECRYPT_ALT
//#define MBEDTLS_ECDH_GEN_PUBLIC_ALT
//#define MBEDTLS_ECDH_COMPUTE_SHARED_ALT
//#define MBEDTLS_ECDSA_VERIFY_ALT
//#define MBEDTLS_ECDSA_SIGN_ALT
//#define MBEDTLS_ECDSA_GENKEY_ALT
```

* File system access
  一些模块包含访问文件系统的功能，但只是对内存缓冲区操作，所以无需替换。

* Real time clock
  一些模块可选择性地访问当前时间，用于测量时间间隔或了解当前绝对时间和日期。每个测量时间间隔的函数都有一个替代版本的代码，以在时间不可用时提供类似的功能（如使用次数来代替时间）。绝对时间和日期只用于X.509检查证书的有效期，如果时间和日期不可用，则跳过此检查。

* Diagnostic output
  Mbed TLS中，只有自测函数（\*\_self_test()）会打印信息（默认使用printf()），可以在include/mbedtls/mbedtls_config.h中禁用。
```C
//#define MBEDTLS_SELF_TEST
```

  也可以在include/mbedtls/mbedtls_config.h中使能MBEDTLS_PLATFORM_PRINTF_ALT，然后在运行时调用mbetls_platform_set_printf()替换为自己的打印函数。或者直接使用MBEDTLS_PLATFORM_FPRINTF_MACRO定义自己的打印函数。

* Platform setup and teardown
  Mbed TLS提供了统一的底层硬件的初始化和去初始化函数，默认不执行任何操作。
  可以在include/mbedtls/mbedtls_config.h禁用默认的函数并重新自己定义。
```C
#define MBEDTLS_PLATFORM_SETUP_TEARDOWN_ALT
```

  新建platform_alt.\*文件，参考platform.\*重新定义mbedtls_platform_context结构体以及mbedtls_platform_setup()和mbedtls_platform_teardown()函数，并分别在Mbed TLS API前后调用。
```C
/**
 * \brief   The platform context structure.
 *
 * \note    This structure may be used to assist platform-specific
 *          setup or teardown operations.
 */
typedef struct mbedtls_platform_context
{
    ...
}
mbedtls_platform_context;

int mbedtls_platform_setup( mbedtls_platform_context *ctx )
{
    ...
    return( 0 );
}

void mbedtls_platform_teardown( mbedtls_platform_context *ctx )
{
    ...
}
```

* PSA cryptography API
  在include/mbedtls/mbedtls_config.h中使能PSA：
```C
#define MBEDTLS_USE_PSA_CRYPTO
```

编译&安装
------
  编译Mbed TLS源码生成库文件，并在系统中安装动态链接库和头文件。
  Mbed TLS目前支持三种编译系统：GNU Make、CMake和Micosoft Visual Studio，其中Make和CMake是主要的开发系统，始终保持完整和最新。
  Make和CMake会生成三个库文件：libmbedcrypto、libmbedx509和libmbedtls，其中libmbedtls依赖于libmbedcrypto和libmbedx509，libmbedx509依赖于libmbedcrypto。所以，在链接的时候如果需要指定顺序，例如使用GNU链接器时顺序如下：
```C
-lmbedtls -lmbedx509 -l mbedcrypto
```

### 在linux环境下编译安装
- make
```shell
make SHARED=1 # SHARED=1生成动态链接库
sudo make install
```

- cmake
```shell
cmake -DUSE_SHARED_MBEDTLS_LIBRARY=On .  # -DUSE_SHARED_MBEDTLS_LIBRARY=On生成动态链接库
make
sudo make install
```

  默认情况下，动态链接库安装到/usr/local/lib目录下，包括libmbedcrypto.so、libmbedx509.so和libmbedtls.so；头文件安装到/usr/local/include/mbedtls目录下；mbedtls相关工具安装到/usr/local/bin目录下，如gen_key等。

### 嵌入式平台
  以STM32为例，手动移植Mbed LTS源码并编译。

* 文件拷贝和工程配置
  将include/和library/两个文件夹拷贝到STM32工程目录下，接着在configs下选择一个配置文件拷贝到include/目录下（如config-suite-b.h）。
  添加源文件（library/\*.c）和包含路径（include/和library/）到工程配置中，添加宏MBEDTLS_CONFIG_FILE指定配置文件（如MBEDTLS_CONFIG_FILE=<config-suite-b.h>）。
  修改config-suite-b.h文件配置功能模块，编译。

使用
===
流程
------
* mbedtls_*_init()

* mbedtls_*_starts()

* mbedtls_*_update()

* mbedtls_*_finish()

* mbedtls_*_free()

示例
------
### 使用HMAC算法生成一个消息认证码
```C
#include "mbedtls/md.h"
void hmac_test(void)
{
  unsigned char secret[] = "a secret"; // 密钥
  unsigned char buffer[] = "some data to hash"; // 消息
  unsigned char digest[32]; // 消息认证码，由于使用SHA算法作为单向散列函数，HMAC计算结果一定为32字节。
  mbedtls_md_context_t sha_ctx = {0};

  mbedtls_md_init(&sha_ctx);
  mbedtls_md_setup(&sha_ctx, mbedtls_md_info_from_type(MBEDTLS_MD_SHA256), 1);
  mbedtls_md_hmac_starts(&sha_ctx, secret, sizeof(secret) - 1); // 设置密钥
  mbedtls_md_hmac_update(&sha_ctx, buffer, sizeof(buffer) - 1); // 填充消息
  mbedtls_md_hmac_finish(&sha_ctx, digest ); // 生成消息认证码
  mbedtls_md_free( &sha_ctx );
}
```

### 使用SHA1算法加密
```C
#include "mbedtls/sha1.h"

void sha1_tet(void)
{
  char *source_cxt = "mculover666";
  char encrypt_cxt[64];
  mbedtls_sha1_context sha1_ctx;

  mbedtls_sha1_init(&sha1_ctx);
  mbedtls_sha1_starts(&sha1_ctx);
  mbedtls_sha1_update(&sha1_ctx, (unsigned char *)source_cxt, strlen(source_cxt));
  mbedtls_sha1_finish(&sha1_ctx, (unsigned char *)encrypt_cxt);
  mbedtls_sha1_free(&sha1_ctx);
}
```

### 使用DES算法对数据加解密
```C
#include "mbedtls/des.h"

void des_tet(void)
{
  char src[16] = "DES algorithm";
  char encrypt[16] = {0};
  char decrypt[16] = {0};
  char iv[8] = {0};
  char key[8] = {4, 7, 1, 0, 4, 7, 7, 0};
  mbedtls_des_context des_ctx;

  mbedtls_des_init(&des_ctx);
  mbedtls_des_setkey_enc(&des_ctx, key);
  memset(iv, 0, sizeof(iv));
  mbedtls_des_crypt_cbc(&des_ctx, MBEDTLS_DES_ENCRYPT, 16, iv, src, encrypt);

  mbedtls_des_setkey_dec(&des_ctx, key);
  memset(iv, 0, sizeof(iv));
  mbedtls_des_crypt_cbc(&des_ctx, MBEDTLS_DES_DECRYPT, 16, iv, src, decrypt);
  mbedtls_des_free(&des_ctx);
}
```

参考
===
* [mbedtls入门和使用](https://blog.csdn.net/weixin_41965270/article/details/88687320)