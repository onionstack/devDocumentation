# MindPlus三方模块-开发者文档
**描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述描述**

## 文件结构
```
└─newExtensions                    // 项目名称
    │  config.json                 // 配置文件
    │  LICENSE.TXT
    │  README.md
    │
    └─arduinoC                     // arduino模式根目录
        │  main.ts                 // 图形块定义以及风格描述
        │
        ├─libraries                // 库文件
        │  └─oled12864
        │          oled12864.cpp
        │          oled12864.h
        │          qrcode.c
        │          qrcode.h
        │
        ├─_images                  // 图片文件
        │      featured.png
        │      icon.svg
        │
        ├─_locales                 // 翻译文件
        │      zh-cn.json
        │      en.json
        │
        └─_menus                   // 下拉菜单参数
                leonardo.json
                uno.json
                nano.json
                mega2560.json
                microbit.json
                mpython.json
```
**config.json:**
```javascript
{
	"name": {
		"zh-cn": "OLED-12864显示屏",
		"en": "OLED-12864 screen"
	},
	"description": {
		"zh-cn": "小体积显示屏，可用于智能穿戴设备的显示器",
		"en": "I2C OLED-2864 Display Module"
	},
	"auther": "dfrobot",
	"email": "xxxxxxxxx@email.com",
	"license": "MIT",
	"isTest": false,
	"isBoard": false,
	"id": "oled12864",
	"platform": ["win", "mac", "web"],
	"asset": {
		"arduinoC": {
			"dir": "arduinoC/",
			"version": "0.0.1",
			"board": ["microbit", "esp32"],
			"main": "oled12864.ts",
			"files": [
				"_locales/zh-cn.json",
				"_images/icon.svg",
				"libraries/oled12864/oled12864.cpp",
				"libraries/oled12864/oled12864.h"
			]
		}
	}
}
```
* name: 模块名称
* description: 模块描述
* auther: 作者
* email: 邮件
* license: 执照
* isTest: True：测试模式，每次编译都会重新构建静态文件(用于调试)，False：只会在第一次编译才会构建静态文件。
* isBoard: 模块是否是主控
* id: 模块id，每个模块都有一个唯一的id
* platform: 选择平台
* asset: 包含多种语言模式
* dir: 指定语言环境
* version: 版本信息
* board: 指定支持的主控
* main: 描述文件的文件名
* files: 包含所需文件的路径，以便加载


## 定义图形块
### 命名空间
```blocks
//% color="#AA278D" iconWidth=50 iconHeight=40
namespace module {
    ...
}
```
创建一个TypeScript命名空间，所有的图形块都写在里面，还可以设置模块的整体颜色以及icon的尺寸，对于图形风格设置应该包含在`//%`描述符中。所有的风格设置不是必须的，如果没有设置这些参数，系统将会按照默认的风格展示，`//%`所包含的内容可以写在一行或多行。
* color: 设置图形块颜色，RGB24位真彩。
* iconWidth: 设置icon显示宽度。
* iconHeight: 设置icon显示高度，icon要求svg格式，图片应该放在`_images`文件夹根目录。

### 图形块
```blocks
//% block="set the line width to [WIDTH] pixels" blockType="command"
export function setBrightness(parameter: any, block: any) {
    ...
}
```
定义一个方形的图形块,`//%`所包含的内容可以写在一行或多行。
* block: 一个图形块的完整描述，也是这个图形块的默认显示语言，`[]`所包含的内容为输入框的名称。
* blockType: 设置外观形状，可选参数有`hat,command,boolean,reporter`

### 输入框
**文本类型**
```blocks
//% block="print [STR]" blockType="command"
//% STR.shadow="string" STR.defl=hello
export function print(parameter: any, block: any) {
    ...
}
```
定义一个文本类型的输入框。
* shadow: 输入框类型。
* defl: 默认参数。

**数字类型**
```blocks
//% block="set wireless channel [NUM]" blockType="command"
//% NUM.shadow="number" NUM.defl=7
export function setWirelessChannel(parameter: any) {
    ...
}
```
定义一个数字类型的输入框。
* shadow: 输入框类型。
* defl: 默认参数。

**范围**
```blocks
//% block="set the line width to [WIDTH] pixels" blockType="command"
//% WIDTH.shadow="range" WIDTH.params.min=1 WIDTH.params.max=128 WIDTH.defl=1
export function setLineWidth(parameter: any, block: any) {
    ...
}
```
定义一个范围限制的输入框。
* shadow: 输入框类型。
* defl: 默认参数。
* params.min: 最小值。
* params.max: 最大值。

**布尔类型**
```blocks
//% block="not [Flag]" blockType="boolean"
//% Flag.shadow="boolean"
export function notTrue(parameter: any) {
    ...
}
```
定义一个波尔类型的输入框。
* shadow: 输入框类型。

**普通下拉菜单**
```blocks
//% block="button [BUTTON] is pressed?" blockType="boolean"
//% Flag.shadow="boolean"
//% BUTTON.shadow="dropdown" BUTTON.options="BTN" BUTTON.defl="BTN.A"
export function buttonIsPressed(parameter: any, block: any) {
    ...
}
```
定义一个下拉菜单。
* shadow: 输入框类型。
* defl: 默认参数。
* options: 下拉菜单选项，在ts文件中定义一个枚举，如果需要区分主控，则需要将引脚定义放在`_menus`文件夹的根目录。

**可输入下拉菜单**
```blocks
//% block="show [STR] on the [LINE] line" blockType="command"
//% STR.shadow="string" STR.defl=hello
//% LINE.shadow="dropdownRound" LINE.options="LINE" LINE.defl="LINE.1"
export function println(parameter: any, block: any) {
    ...
}
```
定义一个下拉菜单，并且可拖入图形块
* shadow: 输入框类型。
* defl: 默认参数。
* options: 下拉菜单选项，在ts文件中定义一个枚举，如果需要区分主控，则需要将引脚定义放在`_menus`文件夹的根目录。

## 描述规则
描述命令必须包含在`//%`描述符中
| 命令  | 含义 | 可选参数 |
| :--- | :---: | :--- |
| color | 设置颜色 | 24位真彩色 |
| iconWidth | icon图标宽度 | 默认30 |
| iconHeight | icon图标高度 | 默认40 |
| block | block描述 | 自定义 例如：xxx[A]xxx |
| blockType | block类型 | hat:帽子形 command:方形 boolean:菱形 reporter:圆形 |
| shadow | 输入框类型 | string:文本 dropdown:下拉不可拖入 dropdownRound:下拉可拖入 boolean:布尔 range:范围 number:数字 |
| defl | 设置默认值 | 自定义 |
| params.min | 用于范围输入类型，设置最小值 | 自定义 |
| params.max | 用于范围输入类型，设置最大值 | 自定义 |
| options | 指定下拉菜单内容 | 自定义 |
