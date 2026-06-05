# 清理C:\Windows\WinSxS 目录
以**管理员身份打开命令提示符 (CMD)**，依次执行：
```
Dism /Online /Cleanup-Image /CheckHealth 
Dism /Online /Cleanup-Image /ScanHealth 
Dism /Online /Cleanup-Image /StartComponentCleanup
```
如需删除旧版更新备份（大幅缩容）：
```
Dism /Online /Cleanup-Image /SPSuperseded
```
