---
date: 2026-06-02
---
# pip 默认配置修改

## 1. 查看当前配置

```powershell
python -m pip config list
```

查看配置文件位置：

```powershell
python -m pip config debug
```

## 2. 设置代理

```powershell
python -m pip config set global.proxy "http://用户名:密码@代理地址:端口"
```

无账号密码时：

```powershell
python -m pip config set global.proxy "http://代理地址:端口"
```

查看代理：

```powershell
python -m pip config get global.proxy
```

删除代理：

```powershell
python -m pip config unset global.proxy
```

## 3. 设置默认源

```powershell
python -m pip config set global.index-url "源地址"
```

例如：

```powershell
python -m pip config set global.index-url "https://pypi.tuna.tsinghua.edu.cn/simple"
```

查看默认源：

```powershell
python -m pip config get global.index-url
```

## 4. 临时指定代理

只本次命令使用，不写入配置：

```powershell
python -m pip install six --proxy "http://用户名:密码@代理地址:端口"
```

## 注意

如果密码中包含特殊字符，需要进行 URL 编码。

常见例子：

```text
@  -> %40
#  -> %23
%  -> %25
:  -> %3A
空格 -> %20
```