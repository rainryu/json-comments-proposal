
# **一个****json****文件的注释提案**
- [前人成果](#前人成果)
- [本提案优点](#本提案优点)
  - [基于json规范约定](#基于json规范约定)
  - [动态类型友好](#动态类型友好)
  - [强类型语言友好](#强类型语言友好)
  - [注释独立于数据部分，便于后续操作](#注释独立于数据部分便于后续操作)
  - [注释与属性最近原则，便于查看与编辑](#注释与属性最近原则便于查看与编辑)
  - [注释与属性层级结构一致](#注释与属性层级结构一致)
- [操作步骤](#操作步骤)
- [实操共3步：](#实操共3步)
- [不足](#不足)


# 前人成果

根据JSON规范(http://www.json.org, RFC 4627, RFC 7159)，不支持注释。JSON规范之所以不允许加注释，主要是防止：过多的注释，影响了文件本身的数据载体的目的。

但是有些场合，尤其是配置文件，还是希望能够帮助说明数据项的含义。一方面有利于描述接口，另一方面能够减少重复性的文档。这在软件快速开发实践中有一定意义。

国科不行民科上

选取几个方案说明

方法一：直接用json-schema，使用规范中的注释字段

> 在json-schema规范中数据结构定义JSON中有一些说明性字段（Annotation），这些字段对应的key有title, description, $comment, default, examples等，可以在这些字段（一般1个即可）中填写某个数据项的含义与用法。这个方案的优点是功能强大，缺点是json-schema与json数据本身还是分离的。
>
> http://json-schema.org/ ,规范网站
>
> https://github.com/epoberezkin/ajv ,著名Javascript实现

出了引文说的问题，最恶心的是官味泛滥，认为注释就必须有“title, description, $comment, default, examples”这些组成，官僚思维

> 方法二：使用JSON5规范
>
> JSON5规范允许在JSON文件中加入注释：单行注释，多行注释均可
>
> 可以使用npm的json5库，用法与JSON库类似。JSON5规范见：https://json5.org/
>
> ![img](https://nqpz6016xx8.feishu.cn/space/api/box/stream/download/asynccode/?code=NjE4NGQyZjU0MTQ2OTZmMDdlYzU2ZTg3OGRkMTliMzNfRkdlT015cjRzMHo1QXFPejZXWWtrN0I3WmFlWmJONEpfVG9rZW46SWlUamIwMnFYb2xwaVZ4YkRmMWNiOWZ2bllmXzE3MjMxNzM1NjI6MTcyMzE3NzE2Ml9WNA)
>
> json5格式直接支持注释

> 方法一：直接用json-schema，使用规范中的注释字段
>
> 在json-schema规范中数据结构定义JSON中有一些说明性字段（Annotation），这些字段对应的key有title, description, $comment, default, examples等，可以在这些字段（一般1个即可）中填写某个数据项的含义与用法。这个方案的优点是功能强大，缺点是json-schema与json数据本身还是分离的。
>
> http://json-schema.org/ ,规范网站
>
> https://github.com/epoberezkin/ajv ,著名Javascript实现

除了引文说的问题，最恶心的是官味泛滥，认为注释就必须有“title, description, $comment, default, examples”这些组成，官僚思维

> 方法二：使用JSON5规范
>
> JSON5规范允许在JSON文件中加入注释：单行注释，多行注释均可
>
> 可以使用npm的json5库，用法与JSON库类似。JSON5规范见：https://json5.org/
>
> ![img](https://nqpz6016xx8.feishu.cn/space/api/box/stream/download/asynccode/?code=NWRmNTdmNDFiOWZiZmY5ZDNlZDQxZDc2NzJjZGJlZjhfYzdzRkgzMVpnN2F3NDBiWWhOenhGZWtWclFuUlZEeGJfVG9rZW46S2g4YWIwam12bzdXakF4ZmdVNGN1bUF1bkNoXzE3MjMxNzM1NjI6MTcyMzE3NzE2Ml9WNA)
>
> json5格式直接支持注释

改规范了还说啥

> 方法三：使用去注释的库
>
> 可以使用npm的strip-json-comments库。支持去掉行注释与块注释，然后再可以用标准的JSON.parse解析strip-json-comments库见：https://github.com/sindresorhus/strip-json-comment
>
> 方法四：使用约定俗成的key作为注释字段
>
> 如以"//"作为注释的key. 但是如果有多个以"//"为key的属性，是否符合协议的？答案是：协议理论上不允许。实现上(几乎?)所有的JS环境都允许，解析之后，只保留最后一项常用的类似key还有： "_comment", "#####"("#"个数自定)等
>
> ![img](https://nqpz6016xx8.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDdlYjg5N2M0YzQwZmVkZGJlNjYwMGY4ZmUzZWM5NjJfTElqVzVkTThkNHZEcG0wZmh5VFRRSXlhblFlcUh6Y0lfVG9rZW46VkdUMGJLODNnb1V2T1B4cmZROWN4RWhCbjhuXzE3MjMxNzM1NjI6MTcyMzE3NzE2Ml9WNA)
>
> 特殊约定的key可以作为注释的标志

卡bug实现

> 方法五：使用重名key作为注释。
>
> 即每个key，使用两次，第1次做注释，第2次做实际属性。原理在方法四中已经介绍：协议理论上不允许。实现上(几乎?)所有的JS环境都允许，解析之后，只保留最后一项
>
> ![img](https://nqpz6016xx8.feishu.cn/space/api/box/stream/download/asynccode/?code=ZmNlZDgxODc2ZjFlNDE4NzUyN2MxNjYzOTEyMWE5MmJfVTNhalAzVkJxU2FUc1J2NWZNcm1SaWEybVBsTWtsSnJfVG9rZW46RDRPVmJNZWpCb2syWkp4bTNYVWNJM1FWbmVKXzE3MjMxNzM1NjI6MTcyMzE3NzE2Ml9WNA)
>
> 同个key使用2次，1次作为注释

也是合卡bug

> 方法六：使用字段key加前缀做注释key
>
> 例如加入属性的key是xyz, 则?xyz作为注释字段。这样的好处是，没有重名的字段，完全符合JSON协议。常用的前缀还有"#", "_", "__"等
>
> ![img](https://nqpz6016xx8.feishu.cn/space/api/box/stream/download/asynccode/?code=OWE5ZmMzYTk1MDZlZTEyYzFiYWM4Mzk5ZjhmZTVmZjdfQ2VjcndMMmgzNGl5dmQ4eFI0M1hGRTNDRHl1ZlZSY2lfVG9rZW46U2g1UGJjMDUyb1Z2cnF4Y0cyeGM3M0xRbmlmXzE3MjMxNzM1NjI6MTcyMzE3NzE2Ml9WNA)
>
> 给key加前缀作为注释的标志

这个基本符合我的要求，在我手搓到这样的时候也认为完美解决了，但工具化操作后发现住宿与属性分离了，不便于查看与编辑

> 方法七：使用支持注释的配置文件管理模块
>
> 如npm中rc库（见：https://github.com/dominictarr/rc），或者config（见：https://github.com/lorenwest/node-config）
>
> 缺点是，只能用于配置相关的Json文件。使用方法需要依照模块的要求。

懒得研究了（捡个鼠标垫配个电脑，捡个蒜头包顿饺子）

# 本提案优点

## 基于json规范约定

- 不违背json规范原则，完全符合json规范
- 不违背json规范原则，完全符合json规范
- 不违背json规范原则，完全符合json规范

## 动态类型友好

- 按规则添加注释即可

## 强类型语言友好

- 按规则拷贝修改即可

## 注释独立于数据部分，便于后续操作

- 生成的json注释隔离在独立的属性下
- 便于工具快速剔除注释

## 注释与属性最近原则，便于查看与编辑

- 注释与属性命名相近，逻辑上在任何编辑器中查看时都是上下相邻的
- 编辑注释时可以就近参考属性原始值，便于注释文案编写，

## 注释与属性层级结构一致

- 注释完全匹配属性的层次结构，结构清晰可读性高

# 操作步骤

1. ## 把model的全部属性直接复制一份，用原属性名后加_作为注释的属性，即原“属性”复制成“属性_”；

1. ## 把属性类型改为字符串类型，即“属性_”全改为字符串类型

1. ## 给model追加一个_json属性作为注释用

# 实操共3步：

以下为一个样例

原始model定义

```C#
public class DeployConfig
    {
        public SetupDemonConfig SetupDemoConfig { get; set; }
        public UpdateDemonConfig UpdateDemoConfig { get; set; }
    }
```

step1

```C#
public class DeployConfig
    {
        public SetupDemonConfig SetupDemoConfig { get; set; }
        public UpdateDemonConfig UpdateDemoConfig { get; set; }
        //1、把model的全部属性直接复制一份，用原属性名后加_作为注释的属性，即原“属性”复制成“属性_”；
        public SetupDemonConfig SetupDemoConfig_ { get; set; }
        public UpdateDemonConfig UpdateDemoConfig_ { get; set; }
    }
```

step2

```C#
public class DeployConfig
    {
        public SetupDemonConfig SetupDemoConfig { get; set; }
        public UpdateDemonConfig UpdateDemoConfig { get; set; }
        //2、把属性类型改为字符串类型，即“属性_”全改为字符串类型
        public string SetupDemoConfig_ { get; set; }
        public string UpdateDemoConfig_ { get; set; }
    }
```

step3

```C#
public class DeployConfig
{
        public SetupDemonConfig SetupDemoConfig { get; set; }
        public UpdateDemonConfig UpdateDemoConfig { get; set; }
        public string SetupDemoConfig_ { get; set; }
        public string UpdateDemoConfig_ { get; set; }
        //3、给model追加一个_json属性作为注释用
        public DeployConfig _json { get; set; }
}
```

生成的json

```JSON
{
  "SetupDemoConfig": {
    "FileInfoList": [
      { "FilePath": "UpdateDemon.exe" },
      { "FilePath": "UpdateDemon.exe.config" },
      { "FilePath": "CopyRun.exe" }
    ],
    "LocalSetupDir": "C:\\Users\\ryu\\Desktop\\cef"
  },
  "UpdateDemoConfig": {
    "DeskShortcut": {
      "ShortcutCmd": "cef.demo.exe",
      "ShortcutIcon": "AppIcon.ico",
      "ShortcutName": ""
    },
    "FileInfoList": [
      { "FilePath": "cef.demo.exe" },
      { "FilePath": "UpdateDemon.exe.config" },
      { "FilePath": "CopyRun.exe" },
      { "FilePath": "AppIcon.ico" }
    ],
    "UpdateVersion": ""
  },
  "_json": {
    "SetupDemoConfig": {
      "FileInfoList": [
        {
          "FilePath": "UpdateDemon.exe",
          "FilePath_": "文件路径"
        }
      ],
      "FileInfoList_": "文件信息列表",
      "LocalSetupDir": "C:\\Users\\ryu\\Desktop\\cef",
      "LocalSetupDir_": "本地安装目录"
    },
    "SetupDemoConfig_": "安装设置",
    "UpdateDemoConfig": {
      "DeskShortcut": {
        "ShortcutCmd": "cef.demo.exe",
        "ShortcutCmd_": "快捷方式命令",
        "ShortcutIcon": "AppIcon.ico",
        "ShortcutIcon_": "快捷方式图标",
        "ShortcutName": "",
        "ShortcutName_": "快捷方式名称"
      },
      "DeskShortcut_": "桌面快捷方式",
      "FileInfoList": [
        {
          "FilePath": "cef.demo.exe",
          "FilePath_": "文件路径"
        }
      ],
      "FileInfoList_": "更新文件列表",
      "UpdateVersion": "",
      "UpdateVersion_": "更新版本"
    },
    "UpdateDemoConfig_": "更新设置"
  }
}
```

# 不足

- 对强类型语言存在属性冗余
- 生成的json文件可能由于部分属性存在嵌套和列表的结构导致出现大量重复注释，手工删除即可
