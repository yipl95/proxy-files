[TOC]

## 依赖

Wails 有许多安装前需要的常见依赖项：

- Go 1.18+ [下载GO](https://go.dev/dl/)
- NPM (Node 15+)  [下载NPM](https://nodejs.org/en/download/)





## 安装wails

```go
go install github.com/wailsapp/wails/v2/cmd/wails@latest
```



### 哦豁，报错了！

<img src="https://gitee.com/yipeilin/drawing-bed/raw/master/drawing-bed/image-20240429140505114.png" alt="image-20240429140505114" style="zoom:67%;" />



> 查看了github的issue，找到问题，好吧，真墙👍🏻，配置后OK了

![image-20240429140620537](https://gitee.com/yipeilin/drawing-bed/raw/master/drawing-bed/image-20240429140620537.png)

```go
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```



### 安装完成后输入命令 `wails doctor` 

输出如下，我们可以快乐的创建项目了

![image-20240429141442364](https://gitee.com/yipeilin/drawing-bed/raw/master/drawing-bed/image-20240429141442364.png)





## 创建项目

### 使用 [模板](https://wails.io/zh-Hans/docs/next/community/templates) 创建项目

当前项目使用的前端框架是 `React + TS` ，你可以根据自己的情况选择合适的模板

```go
wails init -n proxy-files -t react-ts
```

![image-20240429152129906](https://gitee.com/yipeilin/drawing-bed/raw/master/drawing-bed/image-20240429152129906.png)



### 项目结构

```go
.
├── build/                # Wails build 构建产物
│   ├── appicon.png       # App icon
│   ├── darwin/           # MacOS build files
│   └── windows/          # Windows build files
├── frontend/             # 前端页面交互
├── go.mod                # Go module 文件
├── go.sum                # Go module 校验文件
├── main.go               # Wails config 主应用
└── wails.json            # Wails config 项目配置
```

#### 概要：

-  ` app.go` 编写核心业务逻辑，本地开发时会实时编译到 ·`/frontend/wailsjs/go/main/App` `/frontend/src/app.tsx` 中，引入方式如下：

  ```tsx
  import { Greet } from "../wailsjs/go/main/App";
  ```

- `frontend/` 前端页面交互

- `build/` 为项目打包配置&产物，开发过程中不用关心



## 运行项目

### 又报错了！

![image-20240429143633991](https://gitee.com/yipeilin/drawing-bed/raw/master/drawing-bed/image-20240429143633991.png)



>  看错误日志，好像是npm缓存问题，按照输出提示执行命令，再次`wails dev` 项目正常启动

```go
sudo chown -R 501:20 "/Users/linzi/.npm"
```



启动成功，不得不说启动有点慢，可能是我的mac该退休了吧！

![image-20240429144150163](https://gitee.com/yipeilin/drawing-bed/raw/master/drawing-bed/image-20240429144150163.png)

### 本地调试

你也可以在浏览器中调试页面

![image-20240429154314296](https://gitee.com/yipeilin/drawing-bed/raw/master/drawing-bed/image-20240429154314296.png)



## 打包

> 在项目根目录，运行 `wails build` 即可打包当前环境下的应用程序。但是在开发模式下，已经有了一些缓存文件，可以配合 `-clean` 来清理 `build/bin` 目录：

```bash
wails build -clean
```



### 打包 macOS App：

```bash
wails build -platform=darwin/amd64
```



### 打包 Windows 程序：

```bash
wails build -platform=windows/amd64
```



打包文件在 `build/bin` 目录下

![image-20240429155054412](https://gitee.com/yipeilin/drawing-bed/raw/master/drawing-bed/image-20240429155054412.png)
