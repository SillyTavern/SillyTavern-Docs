---
order: 10
label: Windows
---
# Windows 安装

## 使用 Git 进行安装
1. 安装 [NodeJS](https://nodejs.org/en) （建议使用最新的LTS版本）
2. 安装 [Git for Windows](https://gitforwindows.org/)
3. 打开Windows资源管理器（`Win+E`）
4. 浏览到或创建一个不受 Windows 控制或监控的文件夹。（例如：C:\MySpecialFolder\）
5. 在该文件夹内打开命令提示符，方法是点击顶部的 "地址栏"，输入 `cmd`，然后按下 Enter 键。
6. 一旦黑色框（命令提示符）弹出，请在其中输入以下命令之一，然后按下 Enter 键：

- 安装稳定版本：`git clone https://github.com/SillyTavern/SillyTavern -b release`
- 安装开发版本：`git clone https://github.com/SillyTavern/SillyTavern -b staging`

7. 等待 Git 克隆完成后，双击"Start.bat"以使NodeJS安装需求。
8. 全部安装完成后服务将会启动，SillyTavern 将会在您的浏览器中自动弹出。

## 通过 SillyTavern Launcher 安装
1. 安装[Git for Windows](https://gitforwindows.org/)
2. 打开Windows资源管理器（`Win+E`）创建或选择一个您想要安装启动器的文件夹
3. 在该文件夹中打开命令提示符，方法是点击顶部的"地址栏"，输入`cmd`，然后按 Enter 键。
4. 当您看到一个黑色框时，输入以下命令：`git clone https://github.com/SillyTavern/SillyTavern-Launcher.git`
5. 双击`installer.bat`并选择您想要安装的内容
6. 安装完成后，双击`launcher.bat` 启动。

## 通过 GitHub Desktop 安装
（这只允许在 GitHub Desktop 中使用 Git，如果您也想在命令行中使用 Git，您还需要安装[Git for Windows](https://gitforwindows.org/)）
  1. 安装[NodeJS](https://nodejs.org/en)（推荐使用最新的 LTS 版本）
  2. 安装[GitHub Desktop](https://central.github.com/deployments/desktop/desktop/latest/win32)
  3. 安装完 GitHub Desktop 后，点击`Clone a repository from the internet....`（注意：您**不需要**为此步骤创建 GitHub 账号）

  ![image](https://github.com/SillyTavern/SillyTavern-Docs/assets/18619528/fae5d105-449a-4e4a-b679-238347647054)

  4. 在菜单中，点击 URL 选项卡，输入此URL `https://github.com/SillyTavern/SillyTavern`，然后点击 Clone。您可以更改路径来改变 SillyTavern 储存的位置。

  ![image](https://github.com/SillyTavern/SillyTavern-Docs/assets/18619528/6bdded9b-e182-4cac-89ff-e4727dbb97b2)

  6. 要打开 SillyTavern，请使用 Windows 资源管理器浏览到您克隆存储库的文件夹。默认情况下，存储库将被克隆到这里：`C:\Users\[您的Windows用户名]\Documents\GitHub\SillyTavern`
  
  7. 双击`start.bat`文件。（注意：文件名中的`.bat`部分可能会被您的操作系统隐藏，这种情况下，它看起来像一个名为“`Start`”的文件。这是您双击来运行SillyTavern的文件）

  ![image](https://github.com/SillyTavern/SillyTavern-Docs/assets/18619528/a77b8bc2-72a9-42a9-8aa9-1ed89f9bbf35)

  8. 双击后，一个大的黑色命令控制台窗口应该打开，SillyTavern 将开始安装它需要的东西。
  
  9. 安装完成后，如果一切正常，命令控制台窗口应该是这样的，您的浏览器中应该打开了一个 SillyTavern 标签：

  ![image](https://github.com/SillyTavern/SillyTavern-Docs/assets/18619528/d9da4608-94cd-447c-bde2-7f0f9de1c2eb)

  10. 连接到任何[支持的API](https://docs.sillytavern.app/usage/api-connections/)，开始聊天吧！