## Linux系统相关问题

### 用户／权限相关

#### 改变文件权限的命令

chmod

#### 如何实现仅改变目录下所有目录的权限

```Bash
// 改变所有目录权限为755(drwxr-xr-x):
find /opt/lampp/htdocs -type d -exec chmod 755 {} \;
// 改变所有文件权限为644(-rw-r--r--)
find /opt/lampp/htdocs -type f -exec chmod 644 {} \;
```





