# 1. FM Towns 游戏提示请把userdisk放进driver0是什么意思，我在用Tsugaru模拟器，怎么做才好

“请把 userdisk 放进 driver0” 的意思是：FM Towns 游戏需要一张用于存档的 “用户软盘”（User Disk），但你在 Tsugaru 模拟器中没有为 0 号软驱（FD0）挂载虚拟软盘镜像。
一、什么是 User Disk？
作用：相当于游戏存档卡，FM Towns 游戏用它存进度、设置和高分。
格式：标准 3.5 寸软盘镜像（.img / .bin），1232KB 最常用。
二、Tsugaru GUI 解决步骤（推荐）
打开 Tsugaru GUI
运行 Tsugaru_GUI.exe
创建新虚拟软盘（第一次）
菜单：File → New → Floppy disk
选 1232KB → 保存（如 save.img）
挂载到 Drive 0
在 Floppy/User Disk 区：
FD0 → 点 … → 选刚创建的 .img
加载游戏并启动
CD 处选游戏镜像 → 点 Start
提示消失，可正常存档
三、命令行（CUI）启动方法
如果用命令行，加 -FD0 参数：
plaintext
Tsugaru_CUI.exe -CD "游戏.cd" **-FD0 "存档.img"**
四、常见问题
还是提示？
确认是 FD0（Drive 0） 不是 FD1
镜像必须是 1232KB（别用 1.44MB）
重启模拟器
找不到现成镜像？
直接在 Tsugaru 里 新建 最稳妥
五、总结
一句话解决：在 Tsugaru 的 FD0 位置，新建 / 挂载一个 1232KB 的虚拟软盘镜像，再启动游戏即可。
要不要我帮你生成一个可直接使用的 1232KB FM Towns 虚拟软盘镜像 文件？