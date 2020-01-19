# MindPlus-第三方扩展-开发者文档
**Mind+是一款基于Scratch3.0开发的青少年编程软件，支持arduino、micro:bit、掌控板等各种开源硬件，只需要拖动图形化程序块即可完成编程，还可以使用python/c/c++等高级编程语言，让大家轻松体验创造的乐趣。**

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
* email: 邮件,当版本更新需要修改用户库或用户反馈,通过邮件通知开发者
* license: 执照
* isTest: True：测试模式，每次编译都会重新构建静态文件(用于调试)，False：只会在第一次编译才会构建静态文件。建议开发者在调试过程中设置为True,在发布时设置为False。
* isBoard: 模块是否是主控,暂时保留，用于后期扩展主板
* id: 模块id，每个模块都有一个唯一的id
* platform: 选择平台
* asset: 包含多种语言模式
* dir: 指定语言模式路径，```/```不能遗漏，例如：```"dir": "arduinoC/"```
* version: 版本信息，三个数字，从小到大，例如：```"0.0.1"```
* board: 指定支持的主控，[主控列表](#主控列表)
* main: 描述文件的文件名，需要是ts后缀文件，名称自定义，需要在指定路径，例如：```main.ts```
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
描述命令必须包含在`//%`描述符中。

| 命令  | 含义 | 可选参数 |
| :--- | :---: | :--- |
| color | 设置颜色 | 24位真彩色 |
| iconWidth | icon图标宽度 | 默认40 |
| iconHeight | icon图标高度 | 默认40 |
| block | block描述 | 自定义 例如：xxx[A]xxx |
| blockType | block类型 | hat:帽子形 command:方形 boolean:菱形 reporter:圆形 |
| shadow | 输入框类型 | string:文本 dropdown:下拉不可拖入 dropdownRound:下拉可拖入 boolean:布尔 range:范围 number:数字 |
| defl | 设置默认值 | 自定义 |
| params.min | 用于范围输入类型，设置最小值 | 自定义 |
| params.max | 用于范围输入类型，设置最大值 | 自定义 |
| options | 指定下拉菜单内容 | 自定义 |
| block | 指定当前图形块支持的主控或套件 | arduino、leonardo、microbit、esp32、mega2560、vortex、romeo、 arduinounor3、maxbot、maqueen、max、arduinonano|

## Generator对象
Generator是一个提供注册生成代码api的工具，它被内置在mindplus解释器中，可以通过```Generator.```来调用。
```blocks
addInclude(id, code, coverage)
```
在全局区添加include，例如：```Generator.addInclude("Mylibraray", "Mylibraray.h", True) => "#include <Mylibraray.h>"```
* id: 标识符，
* code: 代码
* coverage: 是否覆盖具有相同id的代码， 默认值为False

```blocks
addObject(id, type, code, coverage)
```
在全局区添加对象，例如：```Generator.addObject(`libraray`, `MY_Libraray`, `libraray;`);```
* id: 标识符
* type: class名称
* code: 对象名称
* coverage: 是否覆盖具有相同id的代码， 默认值为false

```blocks
addSetup(id, code, coverage)
```
在setup中添加代码。
* id: 标识符
* code: 代码
* coverage: 是否覆盖具有相同id的代码， 默认值为false

```blocks
addCode(code)
```
在setup或者loop中添加代码，没有返回值的代码添加（一般为方形，帽子形）。
* code: 代码。

```blocks
addCode([code, level])
```
在setup或者loop中添加代码，有返回值的代码添加（一般为圆形，菱形）。
* code：需要注册的代码。
* level：[运算符优先级](#运算符优先级)，为代码添加括号。建议默认设置为```Generator.ORDER_UNARY_POSTFIX```，例如：```Generator.addCode([libraray.read()", Generator.ORDER_UNARY_POSTFIX]);```

```blocks
addEvent(id, type, nam, args, coverage)
```
全局区定义一个回调函数
* id: 标识符
* type: 返回值类型，遵从C++语言规范。lock
* name: 函数名，遵从C++语言规范。
* args: 函数参数，遵从C++语言规范。
* coverage: 是否覆盖具有相同id的代码， 默认值为false

**示例**
```javascript
//% block="when press [BUTTON]" blockType="hat"
//% BUTTON.shadow="range" BUTTON.params.min=0 BUTTON.params.max=15 WIDTH.defl=2
export function myBlock(parameter: any, block: any) {
    let button = parameter.BUTTON.code;
    Generator.addInclude('Mylibraray', '#include <Mylibraray.h>');
    Generator.addObject(`libraray`, `MY_Libraray`, `libraray;`);
    Generator.addEvent("functionName", "String", "functionName", "String message, int8_t error", true);
    Generator.addSetup("libraray.begin", `libraray.begin(${button});`);
    Generator.addSetup(`libraray.callback`, `libraray.callback(functionName);`);
    Generator.addCode('libraray.start();')
}
```
```parameter```是一个包含所有输入参数的对象，通过```parameter.INPUTNAME.code```来获取输入框内容，格式为```parameter.输入框名称.code```；```block```是一个包含图形块当前所有状态的对象，一般用于调试，可以通过```console.log(block)```来查看详细信息。这两个参数如果不需要可以不写。
效果：
```cpp
/*!
 * MindPlus
 * microbit
 *
 */
#include <Mylibraray.h>
// 函数声明
String functionName(String message, int8_t error);
// 创建对象
MY_Libraray libraray;


// 主程序开始
void setup() {
    libraray.begin(2);
    libraray.callback(functionName);
    libraray.start();
}
void loop() {

}

// 事件回调函数
String functionName(String message, int8_t error) {

}
```
## 运算符优先级
| 命令  | 级别 | 适用范围 |
| :--- | :---: | :--- |
| ORDER_UNARY_POSTFIX | 1 | expr++ expr-- () [] . |
| ORDER_UNARY_PREFIX | 2 | -expr !expr ~expr ++expr --expr |
| ORDER_MULTIPLICATIVE | 3 | * / % ~/ |
| ORDER_ADDITIVE | 4 | + - |
| ORDER_SHIFT | 5 | << >> |
| ORDER_RELATIONAL | 6 | >= > <= < |
| ORDER_EQUALITY | 7 | == != === !== |
| ORDER_BITWISE_AND | 8 | & |
| ORDER_BITWISE_XOR | 9 | ^ |
| ORDER_BITWISE_OR | 10 | \| |
| ORDER_LOGICAL_AND | 11 | && |
| ORDER_LOGICAL_OR | 12 | \|\| |
| ORDER_CONDITIONAL | 13 | expr ? expr : expr |
| ORDER_ASSIGNMENT | 14 | = *= /= ~/= %= += -= <<= >>= &= ^= |= |
* 数字越小级别越高

## 主控列表
| 主控 | 类型 | 名称 |
| :--- | :---: | :--- |
| UNO | 主控  | arduino |
| Nano | 主控 | arduinonano |
| Leonardo | 主控 | leonardo |
| Micro:Bit | 主控 | microbit |
| 掌控 | 主控 | esp32 |
| Mega2560 | 主控 | mega2560 |
| Vortex | 套件 | vortex |
| Romeo | 套件 | romeo |
| UNOR3 | 套件 | arduinounor3 |
| Max:Bot | 套件 | maxbot |
| 麦昆 | 套件 | maqueen |
| Max | 套件 |  max |

## 描述参考
</head>
<body link='blue' vlink='purple' class='x22'>

<table border='0' cellpadding='0' cellspacing='0' width='700' style='border-collapse: 
 collapse;table-layout:fixed;width:525pt'>
 <col class='x21' width='87' style='mso-width-source:userset;width:65.25pt'>
 <col class='x22' width='66' style='mso-width-source:userset;width:49.5pt'>
 <col class='x22' width='57' style='mso-width-source:userset;width:42.75pt'>
 <col class='x22' width='72' span='3' style='mso-width-source:userset;width:54pt'>
 <col class='x22' width='67' style='mso-width-source:userset;width:50.25pt'>
 <col class='x22' width='100' style='mso-width-source:userset;width:75pt'>
 <col class='x22' width='107' style='mso-width-source:userset;width:80.25pt'>
 <tr height='19' style='mso-height-source:userset;height:14.25pt' id='r0'>
<td rowspan='2' height='36' class='x26' width='87' style='border-right:1px solid windowtext;border-bottom:1px solid windowtext;height:27pt;width:65.25pt;'>参数</td>
<td rowspan='2' height='36' class='x23' width='66' style='border-right:1px solid windowtext;border-bottom:1px solid windowtext;height:27pt;width:49.5pt;'>命名空间</td>
<td rowspan='2' height='36' class='x23' width='57' style='border-right:1px solid windowtext;border-bottom:1px solid windowtext;height:27pt;width:42.75pt;'>图形块</td>
<td colspan='6' class='x23' width='490' style='border-right:1px solid windowtext;border-bottom:1px solid windowtext;'>输入框</td>
 </tr>
 <tr height='19' style='mso-height-source:userset;height:14.25pt' id='r1'>
<td class='x24'>文本类型</td>
<td class='x24'>数字类型</td>
<td class='x24'>范围类型</td>
<td class='x24'>布尔类型</td>
<td class='x24'>普通下拉菜单</td>
<td class='x24'>可拖入下拉菜单</td>
 </tr>
 <tr height='27' style='mso-height-source:userset;height:20.25pt' id='r2'>
<td height='25' class='x27' style='height:18.75pt;'>color</td>
<td class='x31'>○</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
 </tr>
 <tr height='27' style='mso-height-source:userset;height:20.25pt' id='r3'>
<td height='25' class='x28' style='height:18.75pt;'>iconWidth</td>
<td class='x31'>○</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
 </tr>
 <tr height='27' style='mso-height-source:userset;height:20.25pt' id='r4'>
<td height='25' class='x29' style='height:18.75pt;'>iconHeight</td>
<td class='x31'>○</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
 </tr>
 <tr height='27' style='mso-height-source:userset;height:20.25pt' id='r5'>
<td height='25' class='x29' style='height:18.75pt;'>block</td>
<td class='x31'>‐</td>
<td class='x31'>●</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
 </tr>
 <tr height='27' style='mso-height-source:userset;height:20.25pt' id='r6'>
<td height='25' class='x29' style='height:18.75pt;'>blockType </td>
<td class='x31'>‐</td>
<td class='x31'>○</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
 </tr>
 <tr height='27' style='mso-height-source:userset;height:20.25pt' id='r7'>
<td height='25' class='x29' style='height:18.75pt;'>shadow</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>●</td>
<td class='x31'>●</td>
<td class='x31'>●</td>
<td class='x31'>●</td>
<td class='x31'>●</td>
<td class='x31'>●</td>
 </tr>
 <tr height='27' style='mso-height-source:userset;height:20.25pt' id='r8'>
<td height='25' class='x29' style='height:18.75pt;'>defl</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>○</td>
<td class='x31'>○</td>
<td class='x31'>○</td>
<td class='x31'>○</td>
<td class='x31'>○</td>
<td class='x31'>○</td>
 </tr>
 <tr height='27' style='mso-height-source:userset;height:20.25pt' id='r9'>
<td height='25' class='x29' style='height:18.75pt;'>params.min</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>○</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
 </tr>
 <tr height='27' style='mso-height-source:userset;height:20.25pt' id='r10'>
<td height='25' class='x29' style='height:18.75pt;'>params.max</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>○</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
 </tr>
 <tr height='27' style='mso-height-source:userset;height:20.25pt' id='r11'>
<td height='25' class='x29' style='height:18.75pt;'>options</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>●</td>
<td class='x31'>●</td>
 </tr>
 <tr height='27' style='mso-height-source:userset;height:20.25pt' id='r12'>
<td height='25' class='x29' style='height:18.75pt;'>block</td>
<td class='x31'>‐</td>
<td class='x31'>○</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
<td class='x31'>‐</td>
 </tr>
 <tr height='27' style='mso-height-source:userset;height:20.25pt' id='r13'>
<td height='26' class='x32' style='height:19.5pt;'></td>
<td class='x32'></td>
<td class='x32'></td>
<td class='x22'></td>
<td class='x22'></td>
<td class='x22'></td>
<td class='x22'></td>
<td class='x22'></td>
<td class='x22'></td>
 </tr>
 <tr height='19' style='mso-height-source:userset;height:14.25pt' id='r14'>
<td height='19' class='x30' style='height:14.25pt;'></td>
<td class='x25'></td>
<td class='x25'></td>
<td class='x22'></td>
<td class='x22'></td>
<td class='x22'></td>
<td class='x22'></td>
<td class='x22'></td>
<td class='x22'></td>
 </tr>
 <tr height='19' style='mso-height-source:userset;height:14.25pt' id='r15'>
<td height='19' class='x30' style='height:14.25pt;'></td>
<td class='x25'></td>
<td class='x25'></td>
<td class='x22'></td>
<td class='x22'></td>
<td class='x22'></td>
<td class='x22'></td>
<td class='x22'></td>
<td class='x22'></td>
 </tr>
<![if supportMisalignedColumns]>
 <tr height='0' style='display:none'>
  <td width='87' style='width:65.25pt'></td>
  <td width='66' style='width:49.5pt'></td>
  <td width='57' style='width:42.75pt'></td>
  <td width='72' style='width:54pt'></td>
  <td width='72' style='width:54pt'></td>
  <td width='72' style='width:54pt'></td>
  <td width='67' style='width:50.25pt'></td>
  <td width='100' style='width:75pt'></td>
  <td width='107' style='width:80.25pt'></td>
 </tr>
 <![endif]>
</table>
