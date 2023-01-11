# KIRI 模型转换器

## 说明

本转换方案仅针对于 **`OBJ`** 源文件, 支持的格式表如下:

| 格式名称 |
| ---- |
| OBJ |
| FBX |
| STL |
| GLB |
| GLTF |
| PLY |
| XYZ |

<br/>

## 使用方法

### 1. 安装 whl 包

在 Release 页面下载对应平台对应 python 版本的 whl 包，如 mac 就是 macosx, win 就是 win，Linux 就是 manylinux

cp 就是 python 的版本, 选择自己对应使用的 python 版本即可

例如是 macos python3.9, Apple Silicon 系列芯片的就选择:<br/>
**`kiri_convert-1.0.0-cp39-cp39-macosx_11_0_arm64.whl`**

<br/>

### 2. 使用方法

在使用转换 API 前需要先初始化 SDK, 此步需提供对应的平台账号和密码进行鉴权

随后进行对应的 API 调用即可

<br/>

#### 2.1 鉴权:

代码示例:

```python
import kiri_convert
from kiri_convert import AccountNotExistError, AuthenticationError, ExhaustedError, SDKError

# 开始初始化 SDK 进行鉴权
try:
    kiri_convert.init_sdk(
        env=kiri_convert.EnvType.Test,
        account='test123',
        password='1234567'
    )
except AccountNotExistError:
    print('账号不存在!')
except AuthenticationError:
    print('账号密码错误!')
except ExhaustedError:
    print('接口调用次数用尽!')
except SDKError as e:
    print(f'SDK 异常: {e}')
```

请求参数说明:

| 参数名称 | 说明 |
| --- | --- |
| env | SDK 的环境, kiri_convert.EnvType.Test 为测试环境, kiri_convert.EnvType.Prod 为正式环境 |
| account | 平台账号 |
| password | 平台密码 |

此步可能抛出的异常对照表:

| 异常类型 | 说明 | 解决方案 |
| ----- | ----- | -----|
| AccountNotExistError | 账号不存在 | 检查账号信息是否正确 |
| AuthenticationError | 验证的账号或密码错误 | 检查账号信息是否正确 |
| ExhaustedError | 接口的调用次数用尽 | 联系开发人员 |
| SDKError | SDK 初始化失败 | 初始化失败, 联系开发人员 |

<br/>

#### 2.2 转换

代码示例:

```python
# 进行转换操作
try:
    result = kiri_convert.convert(
        source_file='../models/StoneElephant.obj',
        texture_file='../models/StoneElephant.jpg',
        convert_format=kiri_convert.ModelFormat.FORMAT_XYZ,
        save_name='Export',
        save_path='../exports'
    )

    if result == kiri_convert.SUCCESS:
        print('转换完毕!')
    else:
        print('转换失败!')
except SDKError as e:
    print(f'SDK 异常: {e}')
```

请求参数说明:

| 参数名称 | 说明 |
| --- | --- |
| source_file | OBJ 源文件路径 |
| texture_file | 贴图源文件路径 |
| convert_format | 目标转换的格式类型, 以 kiri_convert.ModelFormat.FORMAT_ 开头, 具体格式参照格式支持表格对照, 例如 xyz 格式则对应为 kiri_convert.ModelFormat.FORMAT_XYZ |
| save_name | 输出的模型文件名称, 不需要到后缀名 |
| save_path | 输出的模型文件保存路径 |

⚠️ 注意：OBJ、贴图、MTL 文件最好放在同一个文件路径下，否则需要在模型文件内有具体的路径引用关系
