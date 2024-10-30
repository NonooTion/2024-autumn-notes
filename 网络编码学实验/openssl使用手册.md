# OpenSSL使用手册

本学期密码学原理与实践课程中，实验需要用到OpenSSL库，但是OpenSSL官方手册是英文，且结构复杂，对于初学者来说阅读难度较高，而网络上又缺少系统的OpenSSL库的学习资料，遂编写该使用手册，用于顺利完成实验

# 实验内容

## 实验一 对称加密实验

### DES算法

DES算法密钥长度为64位，明文按64位进行分组

工作模式有：ECB模式，CBC模式,CFB模式和OFB模式等

实验采用openssl中的des.h库下的函数进行加密

#### 数据结构

首先掌握几个des.h中的重要数据结构

```cpp
typedef unsigned char DES_cblock[8];
typedef /* const */ unsigned char const_DES_cblock[8];
```

cblock指的是加密数据块(cipher block)，实际上是一个包含8个无符号字符的数组,正好对应密钥和明文的64位

```cpp
typedef struct DES_ks {
    union {
        DES_cblock cblock;
        /*
         * make sure things are correct size on machines with 8 byte longs
         */
        DES_LONG deslong[2];
    } ks[16];
} DES_key_schedule;
```

DES_ks,即DES_key_schedule，该结构体用于存储DES加密算法的密钥调度信息

ks[16]是一个包含16个元素的数组，每个元素是一个联合体，意味着每个元素可以存储不同类型的数据，但所有元素在同一时间内只能以一种类型存储数据

联合体中有两个成员:

1. DES_cblock 与上面的DES_cblock相同，是一个8个无符号字符的数组
2. DES_LONG deslong[2] 包含两个DES_LONG类型元素的数组，DES_LONG是一个无符号整型,32位

#### 初始化密钥

进行DES加密之前，需要初始化密钥，这有几种不同的方式

1. 不检查密钥安全性

```cpp
void DES_set_key_unchecked(const_DES_cblock *key, DES_key_schedule *schedule);
```

使用DES_set_key_unchecked初始化密钥，不进行密钥的强度和安全性检查

传入参数解析：

​	const_DES_cblock *key 初始密钥

​    DES_key_schedule *schedule 密钥调度信息,后续加密需要使用

2. 检查密钥安全性

```cpp
int DES_set_key_checked(const_DES_cblock *key, DES_key_schedule *schedule);
```

使用DES_set_key_checked初始化密钥，进行密钥的强度和安全性检查

此情况下，返回值有三种情况:

- 0 密钥初始化成功
- -1 奇偶校验失败
- -2 密钥强度过弱

传入参数同上

如果想要使用DES_set_key_checked，推荐使用

```cpp
void DES_string_to_key(const char *str, DES_cblock *key);
```

方法来生成密钥，否则自己选择的密钥可能会出奇偶校验失败或密钥强度过弱的情况



#### 加密

**ECB模式**

```cpp
void DES_ecb_encrypt(const_DES_cblock *input, DES_cblock *output,
                     DES_key_schedule *ks, int enc);
```

使用DES_ecb_encrypt是DES算法ECB模式的加密函数

参数说明：

1. const_DES_cblock *input 传入的明文块
2.  DES_cblock *output 输出的密文块
3. DES_key_schedule *ks 上一步密钥初始化的密钥调度信息
4. enc 用于指示是进行加密操作还是解密操作 值为DES_ENCRYPT(1)为加密,DES_DECRYPT(0)为解密

**CBC模式**

```cpp
void DES_cbc_encrypt(const unsigned char *in, unsigned char *out, long length,
                     DES_key_schedule *ks, DES_cblock *iv, int enc); 
```

**参数说明**

- **`in`**: 输入数据的指针。指向需要加密或解密的数据。
- **`out`**: 输出数据的指针。指向存储加密或解密结果的缓冲区。
- **`length`**: 数据的长度（以字节为单位）。指定要处理的数据长度。
- **`ks`**: 指向 `DES_key_schedule` 结构的指针。这个结构包含了密钥调度信息，用于加密或解密操作。
- **`iv`**: 指向 `DES_cblock` 结构的指针。这个结构包含了初始化向量（IV），在CBC模式下用于生成反馈值。
- **`enc`**: 加密或解密标志。如果为1，则执行加密操作；如果为0，则执行解密操作。

**CFB模式**

```cpp
void DES_cfb_encrypt(const unsigned char *in, unsigned char *out, int numbits,
                     long length, DES_key_schedule *schedule, DES_cblock *ivec,
                     int enc);

```

参数说明

- **`in`**: 输入数据的指针。指向需要加密或解密的数据。
- **`out`**: 输出数据的指针。指向存储加密或解密结果的缓冲区。
- **`numbits`**: 每次反馈的位数。通常为8，表示使用8位CFB模式。
- **`length`**: 数据的长度（以字节为单位）。指定要处理的数据长度。
- **`schedule`**: 指向 `DES_key_schedule` 结构的指针。这个结构包含了密钥调度信息，用于加密或解密操作。
- **`ivec`**: 指向 `DES_cblock` 结构的指针。这个结构包含了初始化向量（IV），在CFB模式下用于生成反馈值。
- **`enc`**: 加密或解密标志。如果为1，则执行加密操作；如果为0，则执行解密操作。

**OFB模式**

```cpp
void DES_ofb_encrypt(const unsigned char *in, unsigned char *out, int numbits,
                     long length, DES_key_schedule *ks, DES_cblock *iv);
```

参数说明

- **`in`**: 输入数据的指针。指向需要加密或解密的数据。
- **`out`**: 输出数据的指针。指向存储加密或解密结果的缓冲区。
- **`numbits`**: 每次反馈的位数。通常为8，表示按字节进行反馈。
- **`length`**: 数据的长度（以字节为单位）。指定要处理的数据长度。
- **`ks`**: 指向 `DES_key_schedule` 结构的指针。这个结构包含了密钥调度信息，用于加密或解密操作。
- **`iv`**: 指向 `DES_cblock` 结构的指针。这个结构包含了初始化向量（IV），在OFB模式下用于生成反馈值。

