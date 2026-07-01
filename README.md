# QFZN PX4 完整源码包下载说明

这个仓库用来发布一份完整的 QFZN PX4 源码压缩包。

这份压缩包来自下面这个 WSL 目录：

```text
/home/zzynb/code/PX4-Autopilot
```

压缩包里包含 `.git`、子模块信息、隐藏文件、符号链接和 Linux 文件权限。为了避免 Windows 资源管理器复制或解压时报错，这里使用 `.tar.gz` 格式，并且因为文件比较大，已经拆成了几个分卷文件上传到 GitHub Release。

## 一、下载地址

打开这个页面下载：

[QFZN PX4 完整源码包 Release](https://github.com/zheyuuu1/QFZN_px4/releases/tag/archive-20260701-140420)

进入页面后，在 `Assets` 区域下载下面 6 个文件：

```text
PX4-Autopilot-full-20260701-140420.tar.gz.part-000
PX4-Autopilot-full-20260701-140420.tar.gz.part-001
PX4-Autopilot-full-20260701-140420.tar.gz.part-002
PX4-Autopilot-full-20260701-140420.tar.gz.part-003
PX4-Autopilot-full-20260701-140420.tar.gz.sha256
PX4-Autopilot-full-20260701-140420.release-manifest.txt
```

注意：不要只下载 GitHub 页面自动生成的 `Source code (zip)` 或 `Source code (tar.gz)`。那两个不是完整源码包，也不包含这里上传的分卷压缩包。

## 二、下载后要做什么

下载后不需要再“压缩”。正确流程是：

```text
下载全部分卷文件 -> 合并成一个 .tar.gz 压缩包 -> 校验文件是否完整 -> 解压
```

也就是说，`.part-000`、`.part-001`、`.part-002`、`.part-003` 不是单独解压的文件，它们必须先合并成一个完整的：

```text
PX4-Autopilot-full-20260701-140420.tar.gz
```

## 三、推荐解压方式：Linux 或 WSL

推荐在 Linux 或 WSL 里操作。假设 6 个文件都放在同一个目录里，例如 Windows 的下载目录 `Downloads`。

如果你在 WSL 里访问 Windows 下载目录，可以这样进入：

```bash
cd /mnt/c/Users/你的用户名/Downloads
```

例如用户名是 `ZZY10`，就是：

```bash
cd /mnt/c/Users/ZZY10/Downloads
```

然后执行下面命令。

### 1. 合并分卷

```bash
cat PX4-Autopilot-full-20260701-140420.tar.gz.part-* > PX4-Autopilot-full-20260701-140420.tar.gz
```

### 2. 校验文件完整性

```bash
sha256sum -c PX4-Autopilot-full-20260701-140420.tar.gz.sha256
```

如果显示类似下面结果，说明文件完整：

```text
PX4-Autopilot-full-20260701-140420.tar.gz: OK
```

原始压缩包的 SHA256 是：

```text
d3d041bc819c91a167021da6be083ce23ba98a2939db584afd97edb3b86d2bc5
```

### 3. 解压源码包

```bash
tar -xzf PX4-Autopilot-full-20260701-140420.tar.gz
```

解压后会得到目录：

```text
PX4-Autopilot
```

进入目录检查 Git 状态：

```bash
cd PX4-Autopilot
git status
```

## 四、Windows PowerShell 合并方法

如果你先在 Windows PowerShell 里合并，也可以这样做。先进入下载目录：

```powershell
cd $env:USERPROFILE\Downloads
```

合并分卷：

```powershell
cmd /c copy /b PX4-Autopilot-full-20260701-140420.tar.gz.part-000+PX4-Autopilot-full-20260701-140420.tar.gz.part-001+PX4-Autopilot-full-20260701-140420.tar.gz.part-002+PX4-Autopilot-full-20260701-140420.tar.gz.part-003 PX4-Autopilot-full-20260701-140420.tar.gz
```

查看合并后文件的 SHA256：

```powershell
Get-FileHash -Algorithm SHA256 .\PX4-Autopilot-full-20260701-140420.tar.gz
```

输出里的哈希值应该是：

```text
D3D041BC819C91A167021DA6BE083CE23BA98A2939DB584AFD97EDB3B86D2BC5
```

不过，真正解压源码时仍然建议用 WSL 或 Linux 的 `tar -xzf`，这样可以更好地保留 Linux 权限、符号链接和 Git 信息。

## 五、命令行下载方式

如果不想在浏览器里一个个点，也可以在 Linux 或 WSL 里用下面命令下载。

```bash
mkdir -p QFZN_px4_archive
cd QFZN_px4_archive

base_url="https://github.com/zheyuuu1/QFZN_px4/releases/download/archive-20260701-140420"

for file in \
  PX4-Autopilot-full-20260701-140420.tar.gz.part-000 \
  PX4-Autopilot-full-20260701-140420.tar.gz.part-001 \
  PX4-Autopilot-full-20260701-140420.tar.gz.part-002 \
  PX4-Autopilot-full-20260701-140420.tar.gz.part-003 \
  PX4-Autopilot-full-20260701-140420.tar.gz.sha256 \
  PX4-Autopilot-full-20260701-140420.release-manifest.txt
  do
    curl -L -O "$base_url/$file"
  done
```

下载完成后，继续执行前面的合并、校验、解压命令即可。

## 六、编译验证

这份 Release 里的分卷压缩包已经做过一次实际编译验证。验证方式是：重新合并 Release 中的 `.part-*` 文件，校验 SHA256，通过后解压到临时目录，然后删除已有的 `build/cuav_x7pro_default`，重新执行：

```bash
make cuav_x7pro_default
```

验证环境：

```text
WSL Ubuntu-22.04
目标：cuav_x7pro_default
源码 HEAD：fbaf902bbb
验证时间：2026-07-01
```

验证结果：编译通过，并生成：

```text
build/cuav_x7pro_default/cuav_x7pro_default.px4
```

编译日志中的关键结果：

```text
[100%] Built target px4_package
BUILD_EXIT 0
```

注意：本次编译的 FLASH 使用率为 `96.45%`，已经比较接近上限，但当前代码能够完成链接和打包。
## 七、常见问题

### 只下载了一个 `part-000` 可以解压吗？

不可以。必须下载 `part-000` 到 `part-003` 四个分卷，再合并成完整的 `.tar.gz` 文件。

### 为什么不用普通 zip？

这是一个 WSL/Linux 工程目录，里面有 `.git`、子模块、隐藏文件、符号链接和 Linux 权限。Windows 资源管理器直接复制或 zip 这类目录时很容易报错，或者解压后 Git 信息不完整。

### 校验失败怎么办？

一般是某个分卷没有下载完整，或者合并顺序错了。请重新下载 4 个 `.part-*` 文件，并确保文件名没有被浏览器改成 `(1)`、`副本` 之类的名字。

### 解压后怎么看是不是完整？

进入解压出来的目录执行：

```bash
cd PX4-Autopilot
git status
```

能正常显示 Git 状态，就说明 `.git` 信息已经保留下来了。
