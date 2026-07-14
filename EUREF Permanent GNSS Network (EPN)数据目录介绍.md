![image-20260710163554898](./EUREF Permanent GNSS Network (EPN)数据目录介绍-插图/image-20260710163554898.png)

你这个截图是一个**GNSS数据中心根目录（Root Directory）**，从目录结构来看，它是一个提供 **IGS/EPN/CORS类GNSS数据产品** 的服务器。

这些目录基本覆盖了 GNSS 后处理中常用的全部数据：

- 广播星历（Broadcast Ephemeris）
- 高精度星历（Precise Products）
- 观测数据（Observation）
- 实时/准实时数据（NRT）
- 测站信息（Station）
- 产品报告（Reports）

结合你研究的 **PPP / PPP-AR / PPP-RTK**，这个目录非常典型。

下面逐个解释。



# 先说结论：最终汇总表

| 目录        | 英文                | 中文           | 主要内容           | PPP-RTK重要程度 |
| ----------- | ------------------- | -------------- | ------------------ | --------------- |
| BRDC        | Broadcast Ephemeris | 广播星历       | 卫星广播轨道钟差   | ⭐⭐              |
| BRDC_v3     | RINEX3 Broadcast    | RINEX3广播星历 | 多系统广播星历     | ⭐⭐              |
| highrate    | High Rate           | 高频观测       | 1s/5s观测数据      | ⭐⭐⭐             |
| highrate_v4 | High Rate RINEX4    | RINEX4高频数据 | 新格式高速数据     | ⭐⭐⭐             |
| nrt         | Near Real Time      | 准实时数据     | 实时观测           | ⭐⭐⭐⭐            |
| nrt_v3      | NRT RINEX3          | 实时RINEX3     | 实时多系统观测     | ⭐⭐⭐⭐⭐           |
| obs         | Observation         | 观测文件       | 伪距、载波         | ⭐⭐⭐⭐⭐           |
| obs_v3      | RINEX3 Observation  | RINEX3观测     | 现代GNSS观测       | ⭐⭐⭐⭐⭐           |
| products    | Precise Products    | 精密产品       | SP3/CLK/ERP        | ⭐⭐⭐⭐⭐           |
| products_v3 | RINEX3产品          | 新版精密产品   | 多系统精密产品     | ⭐⭐⭐⭐⭐           |
| reports     | Reports             | 报告           | 质量分析           | ⭐⭐              |
| station     | Station Info        | 测站信息       | 坐标、接收机、天线 | ⭐⭐⭐⭐            |

------



# 1. BRDC/

## 全称

$$
\boxed{\text{Broadcast Ephemeris}}
$$

中文：

> 广播星历数据

------

## 数据类型

通常：

```
*.n
*.p
*.nav
```

例如：

```
brdc0010.25n
```

或者：

```
BRDC00IGS_R_20250120000_01D_MN.rnx
```

------

## 内容

来自卫星广播电文：

包括：

### 卫星轨道参数

例如：

$$
a,e,i,\Omega,\omega
$$

### 卫星钟差

$$
a_0,a_1,a_2
$$

用于：

计算卫星位置：

$$
(X_s,Y_s,Z_s)
$$

------

## 用途

主要用于：

- 单点定位SPP
- RTK初始化
- PPP初始计算

------

## 对PPP作用：

一般：

⭐⭐

因为PPP通常使用精密星历。

------

------

# 2. BRDC_v3/

这是：
$$
\boxed{\text{RINEX 3 Broadcast Navigation Data}}
$$
即：

RINEX 3格式广播星历。

支持：

- GPS
- GLONASS
- Galileo
- BDS
- QZSS

例如：

```
BRDC00IGS_R_20250010000_01D_MN.rnx
```

------

相比BRDC：

|          | BRDC   | BRDC_v3 |
| -------- | ------ | ------- |
| 格式     | RINEX2 | RINEX3  |
| 系统     | 较少   | 多系统  |
| 现代应用 | 一般   | 推荐    |

------

# 3. highrate/

## 全称：

$$
\boxed{\text{High Rate Data}}
$$

中文：

> 高频观测数据

------

普通GNSS：

采样间隔：

```
30 s
```

High-rate：

可能：

```
1 s
5 s
```

------

数据：

通常：

```
RINEX OBS
```

例如：

```
ABCD00XXX_R_20250010000_01S_MO.rnx
```

------

用途：

- 地震监测
- 实时形变
- 短时事件分析

例如：

地震发生：

普通30秒：

不够。

1秒数据：

可以捕获。

------

# 4. highrate_v4/

这是：

$$
\boxed{\text{High Rate RINEX Version 4}}
$$

即：

高采样率 + RINEX4格式。

RINEX4是最新标准：

支持：

- 多系统
- 新信号
- 新编码

------

# 5. nrt/

## 全称：

$$
\boxed{\text{Near Real Time}}
$$

中文：

> 准实时数据

------

特点：

数据延迟：

通常：

几秒～几十分钟。

------

用途：

实时：

- PPP-RTK
- 网络RTK
- 实时监测

------

例如：

参考站：

```
AUT10120
DUTH1020
```

实时上传：

```
nrt/
```

------

# 6. nrt_v3/

即：

$$
\boxed{\text{Near Real Time RINEX 3}}
$$

区别：

| 目录   | 格式           |
| ------ | -------------- |
| nrt    | 旧格式         |
| nrt_v3 | RINEX3实时数据 |

------

PPP-RTK最关注：

⭐⭐⭐⭐⭐

因为：

PPP-RTK要求：

实时观测。

------

# 7. obs/

## 全称：

$$
\boxed{\text{Observation Data}}
$$

中文：

> GNSS观测数据

这是最重要的数据之一。

------

文件：

```
*.o
*.d
```

例如：

你之前看到：

```
AUT10120xxxx.d
```

就是这里的数据。

------

内容：

### 伪距

```
C1C
C2W
```

### 载波

```
L1C
L2W
```

### 多普勒

```
D1C
```

### 信噪比

```
S1C
```

------

用途：

⭐⭐⭐⭐⭐

PPP核心输入。

------

# 8. obs_v3/

## 全称：

$$
\boxed{\text{RINEX Version 3 Observation}}
$$

现代GNSS最常用。

支持：

GPS：

```
G
```

GLONASS：

```
R
```

Galileo：

```
E
```

BDS：

```
C
```

------

你的非差非组合PPP-RTK：

主要使用：

$$
\boxed{obs_v3}
$$

------

# 9. products/

这是PPP最重要目录。

## 全称：

$$
\boxed{\text{Precise GNSS Products}}
$$

包含：

------

## （1）精密轨道

文件：

```
*.sp3
```

提供：

卫星坐标：

$$
X,Y,Z
$$

精度：

厘米级。

------

## （2）精密钟差

文件：

```
*.clk
```

提供：

卫星钟：

$$
dt^s
$$

------

## （3）ERP

地球自转参数：

```
*.erp
```

------

PPP计算：

必须：

$$
obs+sp3+clk
$$

------

# 10. products_v3/

这是：

$$
\boxed{\text{RINEX3格式精密产品}}
$$

或者：

新版产品目录。

------

通常：

存：

- 多系统精密产品
- 新格式SSR产品

------

PPP-RTK重点关注。

------

# 11. reports/

## 全称：

$$
\boxed{\text{Reports}}
$$

中文：

> 数据质量报告

例如：

包含：

- 数据完整率
- 周跳统计
- 多路径
- 信噪比

------

用于：

数据质量分析。

------

# 12. station/

## 全称：

$$
\boxed{\text{Station Information}}
$$

中文：

> 测站信息

------

包括：

## 测站日志

例如：

```
AUT10120.log
DUTH1020.log
```

内容：

```
MARKER NAME

APPROX POSITION XYZ

RECEIVER

ANTENNA
```

------

用于：

PPP：

天线改正

$$
PCO/PCV
$$

以及：

参考站坐标。

------

# 

|      |      |      |      |      |
| ---- | ---- | ---- | ---- | ---- |
|      |      |      |      |      |
|      |      |      |      |      |
|      |      |      |      |      |
|      |      |      |      |      |
|      |      |      |      |      |
|      |      |      |      |      |
|      |      |      |      |      |
|      |      |      |      |      |
|      |      |      |      |      |
|      |      |      |      |      |
|      |      |      |      |      |
|      |      |      |      |      |

------

# 如果你的目标是：非差非组合 PPP-RTK

你真正需要的数据链：

```
                参考站观测
                    |
              obs_v3 / nrt_v3
                    |
                    ↓

             GNSS参数估计

                    ↑

products/
 |
 ├── SP3  精密轨道
 ├── CLK  精密钟差
 ├── ERP  地球参数
 └── SSR  改正信息


station/
 |
 ├── 接收机
 ├── 天线
 └── 坐标


                    ↓

              PPP-RTK用户定位
```

所以：

**最核心的四个目录：**
$$
\boxed{
obs_v3 + products + station + nrt_v3
}
$$
这四个就是现代 **PPP-RTK实验和论文中最常用的数据来源。**

你现在看到的这个服务器，基本就是一个完整的 GNSS 科研数据平台。对于你后续做非差非组合PPP-RTK算法验证，数据已经非常齐全。





$$\sum_{i=1}^n a_i$$