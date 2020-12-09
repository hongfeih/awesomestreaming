# KSM的开发

KSM在内容提供商的密钥服务器中，是FPS技术的一部分，其代码在服务器平台运行，实现了本章节描述的相关算法。

KSM是播放app和设备之间的联络人。从app到设备的第一个消息包含SPC。设备的OS解析该SPC并产生CKC。KSM将内容密钥加密并传输给设备，Apple设备使用收到的密钥解密内容服务器传来的FPS媒体内容。[向密钥服务器申请内容密钥](fps-app.md#xiang-mi-yao-fu-wu-qi-shen-qing-nei-rong-mi-yao)说明了该SPC交换过程。

## 处理步骤概述

表2-1概括了为支持FPS，密钥服务器及其KSM可能执行的典型动作序列：

| 步骤 | 服务器动作 |
| :--- | :--- |
| 1 | 从运行在Apple设备上的app接收到SPC消息并解析。参见[SPC消息](ksm-dev.md#spc-xiao-xi) |
| 2 | 根据用户证书（AC）检查SPC的证书哈希值 。参见[用应用证书识别你的FPS App](fps-app.md#yong-ying-yong-zheng-shu-shi-bie-ni-de-fps-app) |
| 3 | 解密SPC载荷。参见[SPC载荷解密](ksm-dev.md#spc-zai-he-jie-mi) |
| 4 | 验证Apple设备使用FPS软件的支持版本。参见[协议版本块](ksm-dev.md#xie-yi-ban-ben-kuai) |
| 5 | 解密SPC载荷中的会话密钥和随机值块。参见[解密\[SK...R1\]载荷](ksm-dev.md#jie-mi-skr-1-zai-he) |
| 6 | 检查SPC消息的一致性。参见[会话密钥和随机值一致性块](ksm-dev.md#hui-hua-mi-yao-he-sui-ji-zhi-yi-zhi-xing-kuai) |
| 7 | 加密内容密钥。参见[内容密钥加密](ksm-dev.md#nei-rong-mi-yao-jia-mi) |
| 8 | 组装CKC载荷中的内容。参见表2-10 |
| 9 | 加密CKC载荷。参见[CKC载荷加密](ksm-dev.md#ckc-zai-he-jia-mi) |
| 10 | 组装CKC消息并发送给Apple设备中的app。参见表2-9 |

## 密码公式语法

本章中描述加密过程的公式使用了以下的命名规则：

* 方括号表示加密值。 例如，用**\[Info\]**表示明文**Info**被加密后的加密值。
* $$e(info)_{K}$$表示使用密钥K对纯文本Info进行加密。进一步地，还可以指定加密算法：如 $$RSA e(info)_{K}$$ 表示使用RSA加密，而 $$AES\_CBC_{IV}e(info)_{K}$$ 表示AES-CBC模式加密，初始向量为IV。
* 相似地，$$d(info)_{K}$$表示使用密钥K对加密值Info进行解密，而$$AES\_ECB d(info)_{K}$$ 表示AES-ECB模式解密。

这种语法的例子可以参见[SPC载荷解密](ksm-dev.md#spc-zai-he-jie-mi)。



## SPC和CKC消息

### TLLV块结构

![&#x56FE;2-1 TLLV&#x5757;&#x7ED3;&#x6784;](../../.gitbook/assets/image%20%288%29.png)



## SPC消息体

## SPC载荷

#### SPC载荷解密

### 会话密钥和随机值块（SK..R1）

#### 解密\[SK...R1\]载荷

### 会话密钥和随机值一致性块

### 协议版本块

## 组织CKC消息

### CKC载荷

### 内容密钥加密

### 在CKC正文中加入SPC块

### CKC载荷加密

## 测试 KSM



