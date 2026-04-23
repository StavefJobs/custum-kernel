# Custom Kernel

自定义 Linux 内核编译项目，支持多种架构和配置类型。

## 项目简介

本项目提供 Linux 内核自定义编译配置，通过 GitHub Actions 自动构建不同架构的内核镜像。

### 支持的配置类型

| 配置 | 架构 | 适用场景 |
|------|------|----------|
| `server` | x86_64 | 高性能服务器、网络、高并发 |
| `virt` | x86_64 | KVM/Xen 虚拟化、PCI 穿透 |
| `embedded` | ARM64 (aarch64) | 嵌入式设备、开发板 |
| `riscv` | RISC-V | RISC-V 架构开发板、嵌入式 |

## 功能特性

### 配置文件

- **config-server.yml**: x86_64 服务器优化配置
  - SMP、NUMA 支持
  - ext4/xfs/btrfs 文件系统
  - VirtIO 驱动
  - 网络优化
  - CPU 频率调节

- **config-virt.yml**: x86_64 虚拟化配置
  - KVM/Xen 虚拟化支持
  - VFIO PCI 穿透
  - vhost_net/vhost_scsi
  - Hyper-V 支持
  - VirtIO 全驱动支持

- **config-embedded.yml**: ARM64 嵌入式配置
  - 精简内核
  - 设备树支持
  - EFI Stub
  - ARM64 特定优化

- **config-riscv.yml**: RISC-V 嵌入式配置
  - RISC-V 64位支持
  - SBI 接口
  - RISC-V PMU 性能监控
  - 交叉编译支持

### GitHub Actions 工作流

自动化构建流程：

1. 下载 kernel.org 官方源码
2. 应用自定义配置
3. 编译内核镜像
4. 编译内核模块
5. 生成 Release 并上传产物

## 使用方法

### 1. 手动触发构建

1. 访问 [GitHub Actions](https://github.com/StavefJobs/custum-kernel/actions)
2. 点击 **Build Custom Kernel**
3. 点击 **Run workflow**
4. 填写参数：
   - **Configuration Type**: 选择 server/virt/embedded/riscv
   - **Kernel Version**: 内核版本（如 7.0.1）
5. 点击 **Run workflow**

### 2. 构建产物

编译完成后会自动创建 Release，包含：

| 文件 | 说明 |
|------|------|
| `vmlinux` | ELF 内核镜像（x86_64） |
| `bzImage` | x86 压缩启动镜像 |
| `Image` | ARM64/RISC-V 压缩镜像 |
| `.config` | 内核配置文件 |

### 3. 安装内核

```bash
# 安装 DEB 包
sudo dpkg -i linux-kernel-*.deb

# 或手动安装
sudo cp vmlinux /boot/vmlinuz-$(uname -r)
sudo cp .config /boot/config-$(uname -r)
sudo cp -r modules/lib/modules/* /lib/modules/
sudo update-grub
```

## 版本历史

| 版本 | 日期 | 更新内容 |
|------|------|----------|
| v7.0.1.2 | 2026-04-23 | Release 添加许可证说明 |
| v7.0.1.1 | 2026-04-23 | 添加 RISC-V 配置支持 |
| v7.0.1.0 | 2026-04-23 | 初始版本：server/virt/embedded 配置 |

## 许可证

- **配置文件 & GitHub Actions**: [MIT License](LICENSE)
- **编译后的内核镜像**: [GPLv2](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html)（与 Linux 内核相同）

> 注意：编译内核镜像衍生自 Linux 内核源码，受 GPLv2 许可证约束。你可以使用和分发编译产物，但必须开源你的修改。

## 参与贡献

欢迎提交 Issue 和 Pull Request！

---

Built with GitHub Actions