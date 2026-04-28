## 注：Shizuku支持系统：Android
## 下载并安装shizuku
1. 点击[**下载**](https://apt.izzysoft.de/fdroid/repo/moe.shizuku.privileged.api_1049.apk)，下载安装包（.apk）
2. 点击安装包，**安装**软件
## 激活shizuku
1. 打开**开发人员选项**
	通常是进入**关于**后点击**版本号**5或7次，可能会要求输入密码，不同品牌可能会有区别，视情况而定
2. 进入**开发人员选项**
3. 找到调试一栏中的**USB调试**和**仅充电模式下允许ADB调试**（没有仅充电模式下允许ADB调试可以不开），将其开启
---
### 接下来的步骤会分为两大类，请视自己情况而定
若在调试一栏中找到**无线调试**，可以跳转到[**下一章节**](https://blog.realityme.top/2025/04/12/shizuku%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8C%97/#%E9%80%9A%E8%BF%87%E6%97%A0%E7%BA%BF%E8%B0%83%E8%AF%95%E5%90%AF%E5%8A%A8)
若没有无线调试，请准备一台搭载**Windows系统的电脑**并继续往下看
## 通过电脑启动（无无线调试，适用于Windows）
1. 下载[**ADB调试工具**](https://googledownloads.cn/android/repository/platform-tools-latest-windows.zip)
2. 解压缩后将解压出来的文件夹重命名为adb并移动到C盘根目录（非必需，但是推荐这么做）
3. 进入电脑**设置**，搜索**高级系统设置**并进入
4. 选择**环境变量**
5. 点击**系统变量**一栏中的**path**并双击进行编辑
6. 点击右侧**新建**
7. 输入刚才的**文件路径**（C:\adb）
8. 然后一路确定，**保存并退出**
	![adb](/images/adb.webp)
9. 接下来将手机（或其他设备）连接电脑，
10. 打开**cmd**（直接在搜索栏输入并回车），输入<code>adb devices</code>，此时若正确开起了开发人员选项中的USB调试，此时设备上可能会出现**是否允许调试**的对话框，勾选**总是允许**，接下来可以在通知中心看到USB调试的信息
11. 再次在终端中输入<code>adb devices</code>，如无问题将会看到类似如下内容
~~~
List of devices attached
XXX      device
~~~
12. 复制以下指令并粘贴到cmd中，如无问题你将会在 Shizuku 中看到已启动成功
~~~
adb shell sh /sdcard/Android/data/moe.shizuku.privileged.api/start.sh
~~~
### 该启动方式适用于未 root 且运行 Android 10 及以下版本或者HarmonyOS的设备。很不幸，该启动方式需要连接电脑。且由于系统限制，每次重新启动后都需要再次进行启动步骤
## 通过无线调试启动
1. 进入开发人员选项中的**无线调试**
2. **启用**无线调试
3. 配对（仅需一次）
	1. 在 Shizuku 内开始**配对**
	![shi](https://shizuku.rikka.app/images/start_paring_from_shizuku.webp)
	2. 点按无线调试中的**使用配对码配对设备**
	![zu](https://shizuku.rikka.app/images/start_pairing.webp)
	3. 在Shizuku的**通知**中**填入配对码**
	![ku](https://shizuku.rikka.app/images/enter_pairing_code.webp)
4. 启动Shizuku
![shizukucontact](https://shizuku.rikka.app/images/start_shizuku.webp)
如果无法启动，尝试禁用并启用无线调试
### 通过无线调试启动适用于 Android 11 或以上版本。这种启动方式无需连接电脑。由于系统限制，每次重新启动后都需要再次进行启动步骤
## 通过 root 启动
如果您的设备已经 root，直接启动即可
## 常见问题
许多厂商对 Android 系统进行了修改，这会造成 Shizuku 无法正常工作。

### 通过无线调试启动：一直显示“正在搜索配对服务”
请允许 Shizuku 在后台运行。
搜索配对服务需要访问本地网络，许多厂商在应用不可见后立刻禁止应用访问网络。您可以在网络上搜索如何在您的设备上允许应用在后台运行。

### 通过无线调试启动：点击“输入配对码”后立刻提示失败
MIUI（小米、POCO）
在系统设置的“通知管理”-“通知显示设置”将通知样式切换为“原生样式”。

### 通过无线调试启动/通过连接电脑启动：adb 权限受限
#### MIUI（小米、POCO）
在“开发者选项”中开启“USB 调试（安全设置）”。注意，这和“USB 调试”是两个分开的选项。

#### ColorOS（OPPO & OnePlus）
在“开发者选项”中关闭“权限监控”。

#### Flyme（魅族）
在“开发者选项”中关闭“Flyme 支付保护”。

### 通过无线调试启动/通过连接电脑启动：Shizuku 随机停止
#### 所有设备
保证 Shizuku 可以在后台运行。

不要关闭“USB 调试”及“开发者选项”。

在“开发者选项”中将 USB 使用模式改为“仅充电”。

在 Android 8 上的选项是“选择 USB 配置”-“仅充电”。
在 Android 9 及以上版本上选项是“默认 USB 配置”-“不进行数据传输”。

（Android 11+）启用“停用 adb 授权超时功能”选项

#### EMUI (华为)
在“开发者选项”中开启「“仅充电”模式下允许 ADB 调试选项」。

#### MIUI（小米、POCO）
不要使用“手机管家”的扫描功能，因为它会禁用开发者选项。

#### Sony
不要点击连接 USB 后弹出的对话框，因为这会导致 USB 使用模式发生变化。

### 通过 root 启动：无法开机启动
请允许 Shizuku 在后台运行。
