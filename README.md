# SUDA_RISCV
本文档仅仅记录作者把玩开源FPGA开发板`荔枝糖`的经历。

## 安装 TD 驱动

* Linux环境

  LicheeTang默认已经烧录了JTAG固件（用于烧录FPGA码流到配置芯片中，相当于是USB Blaster）

  ![插入USB后显示的结果](https://s1.ax1x.com/2018/09/27/iQQaFg.png)

* Windows环境

  参考[荔枝糖官方文档—安装TD驱动](http://tang.lichee.pro/get_started/driver.html)



## 烧写 FPGA 码流

![烧写码流](https://s1.ax1x.com/2018/09/27/iQQ4p9.png)

* 注意要选择 `PROGRAM_FLASH` 模式



## 搭建开发环境

### 安装宿主机工具

```bash
sudo apt-get install autoconf automake autotools-dev curl libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev libglib2.0-dev
```

### 编译工具链

```bash
# 添加子模块
git submodule add https://github.com/riscv/riscv-gnu-toolchain.git riscv-gnu-toolchain
# 初始化子模块
git submodule update --init --recursive
# 创建编译目录
mkdir -p toolchains/build
cd toolchains/build
# 配置(32位处理器，支持的指令集为RV32GC，支持的ABI为ilp32)
# ilp32 (32-bit soft-float), ilp32d (32-bit hard-float), ilp32f (32-bit with single-precision in registers and double in memory)
../../riscv-gnu-toolchain/configure --prefix=/home/maoshengrong/SUDA_RISCV/toolchains/ --with-arch=rv32gc --with-abi=ilp32
# 编译
make newlib
# 测试
make report-newlib
```

### 编译qemu虚拟机

```bash
# 创建编译目录
mkdir -p qemu/build
cd qemu/build
# 配置
../../riscv-gnu-toolchain/riscv-qemu/configure --prefix=/home/maoshengrong/SUDA_RISCV/qemu/
# 编译
make -j4
# 安装
make install
```

### 编译OpenOCD

```bash
# 添加子模块
git submodule add https://github.com/riscv/riscv-openocd.git riscv-openocd
# 初始化子模块
git submodule update --init --recursive
```



## 参考文献

1. [riscv-gnu-toolchain仓库](https://github.com/riscv/riscv-gnu-toolchain)
2. [GNU MCU Eclipse 项目主页](https://gnu-mcu-eclipse.github.io/)