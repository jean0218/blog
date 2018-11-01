### 目录结构说明如下，重要的有：

- android/ android 原生代码（使用 android studio 要打开这个目录，如果直接打开父目录报错）
- ios/ ios 原生代码（使用 xcode 打开这个目录，如果直接打开父目录报错）
- index.js 打包 app 时进入 react native（js 部分） 的入口文件（0.49 以后安卓、ios 共用一个入口文件）
- App.js 可以理解为 react native（js 部分） 代码部分的入口文件，比如整个项目的路由在这里导入
### 上面是最重要的四个目录/文件，其他说明如下：

- _test_/ 测试用（暂未使用）
- app.json 项目说明，主要给原生 app 打包用，包括项目名称和手机桌面展示名称 React Native : 0.41 app.json
- package.json 项目依赖包配置文件
- node_modules 依赖包安装目录
- ejg kw yarn.lock yarn 包管理文件
其他配置文件暂时无需改动，在此不做说明

# 启动服务
react-native run-android

### js代码热更新
在模拟器中按ctrl+m，打开菜单，运行Reload


### 按示例代码敲出的问题
#### 0.57.3使用Button报错java.lang.String cannot be cast to 

已确认由于官方合并代码不完整，导致0.57.3的Button组件存在bug，在官方发布0.57.4之前的解决办法（二选一）是

使用Touchable组件代替Button

降级到0.57.1 yarn add react-native@0.57.1 schedule@0.4.0 ，必要的话删除node_modules重新yarn， 以及务必重启packager

const instructions = Platform.select({
  ios: 'Press Cmd+R to reload,\n' + 'Cmd+D or shake for dev menu',
  android:
    'Double tap R on your keyboard to reload,\n' +
    'Shake or press menu button for dev menu',
});
## 疑问：
1. 如何更改app名
1. 如何打包app

最近在敲reactNative的示例代码，脑中又有新的问题涌现
他相对于React来说，虽然很多范式和写法是一样的，但终究有些API是重写过的，
如何实现，一套代码多平台呢。
即，我一个功能写完了后在H5平台运行，我又如何能在安卓中间无缝迁入进来。


### 如果我是前端主管
我就创建一套完整的工作流，从开发部署到上线打包。
也是来这边之后的视野广阔了很多。
项目初始化：学会编写cli，目录结构初始，
	及各类校验规则编写,eslint
编码:各平台统一化思路
基础库沉淀：	
	搭建公司内部组件库系统
	基础库编写，
	API文档系统建立

	页面性能优化流程的点，
	和性能优化调试点
测试环境打包测试：
	单元测试环境建议，单元测试代码示例
	端对端测试
	gitLabCI
	jekins配置
生产环境打包测试
	nginx性能优化
	sentry错误日志监控，如何查看及调试错误日志

 会react再来看reactNative真的是so easy


### 目录结构
 reactNative
 存放reatNative编写的组件
 此APP是为了学习reactNative使用，所以明确可以使用JS的组件不再复用，全采用reactNative实现

### 开发中遇到的问题
1. 安装好js插件后，再运行报命令失效
插入图1
打开翻墙，再次运行run-andriod又可以 ==！