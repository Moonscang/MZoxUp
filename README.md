# MZoxUp README

MZoxUp 是一个面向 x86-64 / ARM64 / RISC-V64 / LoongArch64 的 UEFI & Legacy BIOS 双模式引导器，提供 King / root / admin 三级权限与安全模式救援终端 `MZUsh#king->`，搭配 btrfs 快照实现 秒级系统恢复。

---

1. 核心特性

特性	描述	
双启动	同一份源码生成 `MZoxUp.efi`（UEFI）与 `MZoxUp.mbr`（Legacy BIOS）。	
三级权限	King（固件级）、root（系统只读+配置可写）、admin（普通用户）。	
安全模式	按 F8 或启动失败自动进入，提供图形救援 GUI 与 `MZUsh#king->` 终端。	
btrfs 快照	一键回滚、事务更新、崩溃秒恢复。	
一次性令牌	外部 `AllowWrite.token` 解锁系统卷写权限。	

---

2. 快速开始（x86-64 QEMU）

```bash
git clone --recurse-submodules https://gitee.com/mirrors/edk2.git
cd MZoxUp
make ARCH=x86_64 MODE=uefi   # 生成 MZoxUp.efi
./scripts/qemu-run.sh        # 立即验证
```

---

3. 目录结构

```
MZoxUp/
├── cfg/               # 引导菜单配置
├── docs/              # 文档与截图
├── scripts/           # 构建、烧录、QEMU 脚本
└── src/
    ├── common/        # 通用算法与工具
    ├── arch/          # 按架构分目录（x86_64, arm64…）
    ├── fs/            # FAT32 / btrfs 解析
    ├── gui/           # 安全模式图形界面
    ├── shell/         # MZUsh#king-> 终端
    └── token/         # AllowWrite.token 生成/校验
```

---

4. 权限模型

身份	生效位置	系统卷可写	家目录/配置可写	
King	安全模式	✅ 任意子卷	✅	
root	正常系统	❌ 只读	✅ `/etc /var /opt /home`	
admin	正常系统	❌ 只读	✅ 仅自家目录	

---

5. 真机烧录

```bash
sudo dd if=Build/MZoxUp/RELEASE_X64/MZoxUp.fd \
        of=/dev/sdX bs=4M
# 或
make ARCH=x86_64 MODE=mbr
sudo dd if=MZoxUp.mbr of=/dev/sdX bs=512 count=1
```

---

6. 开发与贡献
- 分支 `dev/<arch>`，PR 前 `make lint` 通过。  
- 里程碑见 `docs/milestones.md`。  

---

7. 许可证
MIT © 2025 Moonscang | ZexoMinx
