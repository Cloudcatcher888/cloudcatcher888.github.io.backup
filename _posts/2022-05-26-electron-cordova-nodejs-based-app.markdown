---
layout: post
title:  "Electron & Cordova: Two Nodejs Based App Development Kit"
date:   2022-05-26
categories: jekyll update
---
# 使用electron开发桌面端应用



[参考](https://www.electronjs.org/zh/docs/latest/tutorial/quick-start)

首先检查nodejs是否安装：node -v

首先创建应用：

```bash
$ mkdir my-electron-app && cd my-electron-app
$ npm init
```

会创建目录，然后进入该目录，类似git一样对目录进行初始化，使之成为electron项目工作目录，注意package.json文件里main应该为main.js。

然后在该目录下安装依赖包，相当于整个app带着依赖包到处走，所以electronapp体积较大。


```bash
$ npm install --save-dev electron
```

此时如果安装卡住是因为网络问题，可以使用china CDN镜像，


```bash
$ ELECTRON_MIRROR="https://npmmirror.com/mirrors/electron/" npm install --save-dev electron
```

在main.js文件中管理整个软件的运行：


```javascript
// main.js

// Modules to control application life and create native browser window
const { app, BrowserWindow } = require('electron')

const createWindow = () => {
  // Create the browser window.
  const mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
  })

  // 加载 index.html
  mainWindow.loadFile('index.html')
  // URL也可以
  //mainWindow.loadURL('http://xxxx')
}

// 这段程序将会在 Electron 结束初始化
// 和创建浏览器窗口的时候调用
// 部分 API 在 ready 事件触发后才能使用。
app.whenReady().then(() => {
  createWindow()
})
```

编译打包成app：（在mac上打包出来是macOS版本，windows上打包出来是Windows版本）


```bash
$ npm install --save-dev @electron-forge/cli
$ npx electron-forge import

✔ Checking your system
✔ Initializing Git Repository
✔ Writing modified package.json file
✔ Installing dependencies
✔ Writing modified package.json file
✔ Fixing .gitignore

We have ATTEMPTED to convert your app to be in a format that electron-forge understands.

Thanks for using "electron-forge"!!!
```


```bash
$ npm run make

> my-electron-app@1.0.0 make /my-electron-app
> electron-forge make

✔ Checking your system
✔ Resolving Forge Config
We need to package your application before we can make it
✔ Preparing to Package Application for arch: x64
✔ Preparing native dependencies
✔ Packaging Application
Making for the following targets: zip
✔ Making for target: zip - On platform: darwin - For arch: x64
```

软件包会在out文件夹里。

另外在开发时可以用start脚本预览，首先在package.json的scripts字段下增加start命令：


```bash
{
  "scripts": {
    "start": "electron ."
  }
}
```

然后用 npm start 运行app。





# 使用Cordova 开发移动端app

参考：[1 基本流程](https://cordova.apache.org/docs/en/11.x/guide/cli/index.html)  [2 IOS平台](https://cordova.apache.org/docs/en/11.x/guide/platforms/ios/index.html)

首先安装cordova：


```bash
$ sudo npm install -g cordova
```

然后在任意目录下创建项目以及项目文件夹：


```bash
$ cordova create hello com.example.hello HelloWorld
```

分别代表文件夹名（hello），项目bundleid（com.example.hello），和项目名称（HelloWorld）。

进入该目录下，例如cd hello。

选择要编译的平台：


```bash
$ cordova platform add ios
```

预先安装需要的SDK，可以使用`cordova requirements` 查看缺少哪些环境，具体可参考[IOS环境](https://cordova.apache.org/docs/en/11.x/guide/platforms/ios/index.html)

IOS平台的编译流程：（注意需要设置team）

1. 首先在根目录下建立build.json的文件，专门设置team信息（还可以设置其他编译配置，但是因为只缺这个，所以只配置这个，而且不一定放在根目录下，可以写全路径）


```json
{
  "ios": {
    "debug": {
      "developmentTeam": "YOURTEAMID" #会在xcode里出现，cordova编译时只要写任意值就行，但是到了xcode里必须填下拉菜单里存在的值
    },
    "release": {
      "developmentTeam": "YOURTEAMID"#会在xcode里出现
    }
  }
}
```

2. 然后编译（其实是转化成ios的代码）：


```bash
$ cordova build --buildConfig build.json
```

3. 进入platforms/ios目录下打开xcode项目文件xcworkspace。这里会出现很多错误：如果untrusted device，就要重连usb，然后手机上信任设备；如果fail running重启手机可能会好；如果General栏提示Bundle Identifier不唯一，就写个新的，例如把中间的换成名字。