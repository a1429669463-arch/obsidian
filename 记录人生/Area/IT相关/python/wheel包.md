---
date: 2026-06-02
---
# 离线安装用 wheel 包准备方法

## 1. 准备 `requirements.txt`

写入需要下载的模块：

```txt
pandas==2.2.3
pywin32==306
openpyxl==3.1.5
pillow==11.0.0
```

## 2. 下载 wheel 包

在 `requirements.txt` 所在目录执行：

```powershell
python -m pip download -r requirements.txt -d wheelhouse
```

执行后会生成 `wheelhouse` 文件夹，里面包含这些模块及其依赖包的安装文件。

## 3. 离线安装

将 `requirements.txt` 和 `wheelhouse` 文件夹复制到目标电脑后执行：

```powershell
python -m pip install --no-index --find-links=wheelhouse -r requirements.txt
```

## 注意

下载 wheel 包的电脑和目标电脑最好保持：

- Python 版本一致
    
- Windows 位数一致，例如都是 64 位

## 相关笔记

- [[pip配置]] — pip 代理和默认源配置
    