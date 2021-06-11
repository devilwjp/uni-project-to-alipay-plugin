# 使用uniapp2wxpack，基于uni-app开发支付宝小程序插件  
此项目是uni-app开发支付宝小程序插件的示例项目，主要展示如何使用uniapp2wxpack的插件开发模式  

## 注意  
项目中涉及appid的部分需要自己修改成项目的实际appid，否则无法开发插件 

## 快速起步  
### 第一步  
准备一个纯净的uni-app项目  

### 第二步  
运行uniapp2wxpack-cli（uniapp2wxpack 3.0以上版本）
```
npx uniapp2wxpack@alipay-plugin --create 
```
cli命令运行完之后，项目同时具备uni-app解耦模式的开发和插件开发能力  
在package.json中分别会生成以下命令  
build:mp-alipay-pack  
dev:mp-alipay-pack  
build:mp-alipay-pack-plugin  
dev:mp-alipay-pack-plugin  
并且会自动创建文件projectToSubPackageConfig.js  
自动创建目录mainWeixinMp（此目录打包后会成为miniprogram目录，也就是插件开发模式的预览小程序）  

### 第三步  
手动在项目根目录创建插件开发需要的project.config.json，并且内容miniprogramRoot和pluginRoot属性按以下填写  
并且自行填写真实的appid  
```json
{
	"miniprogramRoot": "miniprogram/",
	"pluginRoot": "uniSubpackage/",
	"compileType": "plugin",
	"setting": {},
	"appid": "xxxxxxxxx",
	"projectname": "uniapp2wxpack开发支付宝小程序插件",
	"simulatorType": "wechat",
	"simulatorPluginLibVersion": {},
	"condition": {}
}
```
### 第四步  
在src目录下手动创建plugin.json  
并且main属性必须按以下内容填写，也就意味着插件的接口文件指向src/main.js（因为打包后路径会变成common/main.js）  
```json
{
	"publicComponents": {
		"hello-component": "components/test"
	},
	"pages": {
		"hello-page": "pages/index/index"
	},
	"main": "common/main.js"
}
```
### 第五步  
对src/main.js进行js接口的设置（非必须）  
在main.js的最底部填写相关需要的内容  
其中如果需要对第三方小程序公开自定义组件的使用，需要定义vue的全局组件，见如下范例  
插件的接口使用特殊的API __uniPluginExports进行暴露  
```javascript
import Vue from 'vue'
import App from './App'

Vue.config.productionTip = false

App.mpType = 'app'

const app = new Vue({
  ...App
})
app.$mount()


/**
 * 要输出给第三方小程序使用的自定义组件,必须声明全局组件
 * 如果只是单纯的引入,不声明全局组件,build模式会无法正常输出组件
 */
import test from './components/test'
Vue.component('test',test)


// 插件接口输出使用特殊的API
__uniPluginExports = {
    hello: function() {
        console.log('Hello plugin!')
    }
}
```
### 第六步  
运行命令  
```
npm run dev:mp-alipay-pack-plugin （开发模式）
npm run build:mp-alipay-pack-plugin （生产模式）
```
### 第七步  
使用支付宝开发者工具预览dist目录下相关环境的mp-alipay-pack-plugin目录  
