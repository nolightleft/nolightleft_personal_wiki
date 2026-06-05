创建PROFILE文件
```powershell
if(!(Test-Path $PROFILE)){New-Item $PROFILE -Force} 
# 直接打开编辑 
notepad $PROFILE
```

PROFILE文件配置
```powershell
$env:Path += ";C:\Program Files\Vim\vim92"

# ========== Bash风格Tab补全配置 ==========
# Tab：最长自动补齐 + 多结果列出全部（核心）
Set-PSReadLineKeyHandler -Key Tab -Function Complete

# 顺带开启Emacs快捷键(Ctrl+A行首、Ctrl+E行尾、Ctrl+U清行，Linux习惯)
Set-PSReadLineOption -EditMode Emacs
# 上下箭头：按输入内容检索历史(类似bash)
Set-PSReadLineOption -HistorySearchCursorMovesToEnd
Set-PSReadLineKeyHandler -Key UpArrow -Function HistorySearchBackward
Set-PSReadLineKeyHandler -Key DownArrow -Function HistorySearchForward

# 关闭提示滴滴声
Set-PSReadLineOption -BellStyle None
```

立刻生效
```
. $PROFILE
```