# json-comments-proposal
# 一个json文件的注释提案

优点

- 不违背json规范原则，完全符合json规范!

  不违背json规范原则，完全符合json规范!

  不违背json规范原则，完全符合json规范!

- 对于动态类型语言只需要按规则添加注释即可;

- 对于强类型语言仅需要拷贝修改即可满足提案规则;

- 生成的json注释隔离在独立的属性下，编译工具快速剔除注释;

- 注释中的描述与需要注释的属性由于命名原因距离不会太远;

- 注释附近就有原始属性，用于编写样例;

实操共4步：

1. 把model的全部属性直接复制一份作为附加的注释用属性；
2. 将复制的属性名用原属性字后加_的形式作为注释的属性名；
3. 把属性类型改为字符型，如果属性为引用类型，对引用类型的注释可以在引用类型声明处声明；
4. 在最外层model加一个_json属性作为注释用，类型为当前model类型即可；

以下为一个样例

原始model定义

 [DataContract]
    class DeployConfig
    {
        [DataMember(EmitDefaultValue = false)]
        public SetupDemonConfig **SetupDemoConfig** { get; set; }
        [DataMember(EmitDefaultValue = false)]
        public UpdateDemonConfig **UpdateDemoConfig** { get; set; }
    }

{
  "**SetupDemoConfig**": {
    "FileInfoList": [
      { "FilePath": "UpdateDemon.exe" },
      { "FilePath": "UpdateDemon.exe.config" },
      { "FilePath": "CopyRun.exe" }
    ],
    "LocalSetupDir": "C:\\Users\\ryu\\Desktop\\cef"
  },
  "**UpdateDemoConfig**": {
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
  }
}

```c#
 [DataContract]
    class DeployConfig
    {
        [DataMember(EmitDefaultValue = false)]
        public SetupDemonConfig SetupDemoConfig { get; set; }
        [DataMember(EmitDefaultValue = false)]
        public UpdateDemonConfig UpdateDemoConfig { get; set; }
    }
```

```json
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
  }
}
```

加入注释后

[DataContract]
    class DeployConfig
    {
        [DataMember(EmitDefaultValue = false)]
        public SetupDemonConfig SetupDemoConfig { get; set; }
        [DataMember(EmitDefaultValue = false)]
        public UpdateDemonConfig UpdateDemoConfig { get; set; }
        [DataMember(EmitDefaultValue = false)]
        public string **SetupDemoConfig_** { get; set; }
        [DataMember(EmitDefaultValue = false)]
        public string **UpdateDemoConfig_** { get; set; }
        [DataMember(EmitDefaultValue = false)]
        public DeployConfig **_json** { get; set; }
}

```
 [DataContract]
    class DeployConfig
    {
        [DataMember(EmitDefaultValue = false)]
        public SetupDemonConfig SetupDemoConfig { get; set; }
        [DataMember(EmitDefaultValue = false)]
        public UpdateDemonConfig UpdateDemoConfig { get; set; }

        [DataMember(EmitDefaultValue = false)]
        public string SetupDemoConfig_ { get; set; }
        [DataMember(EmitDefaultValue = false)]
        public string UpdateDemoConfig_ { get; set; }

        [DataMember(EmitDefaultValue = false)]
        public DeployConfig _json { get; set; }

}
```

生成的json

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
  "**_json**": {
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
    "**SetupDemoConfig_**": "安装设置",
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
    "**UpdateDemoConfig_**": "更新设置"
  }
}

```
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
