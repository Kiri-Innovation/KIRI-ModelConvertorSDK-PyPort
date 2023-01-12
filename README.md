# KIRI 3DModel file formats convertor

## Description

This convertor only works on .OBJ file as input. The following is a list of file formats available for converting:

| File format names |
| ---- |
| OBJ |
| FBX |
| STL |
| GLB |
| GLTF |
| PLY |
| XYZ |

<br/>

## Usage

### 1. install .whl package

Please download correct .whl package in Release page. Mac please use macosx package, windows use win package and linux use manylinux package.

cp stands for python version, please use same version as your python on your platform.

If you are using Apple Silicon, please select:<br/>
**`kiri_convert-1.0.0-cp39-cp39-macosx_11_0_arm64.whl`**

<br/>

### 2. Usage

Please initialize the SDK before using KIRI 3DModel file formats convertor API. 

Pass the authentication with your account and password and then you will be able to call the API

<br/>

#### 2.1 Authentication:

Code examples:

```python
import kiri_convert
from kiri_convert import AccountNotExistError, AuthenticationError, ExhaustedError, SDKError

# Initialize the SDK and authenticate
try:
    kiri_convert.init_sdk(
        env=kiri_convert.EnvType.Test,
        account='test123',
        password='1234567'
    )
except AccountNotExistError:
    print('Account does not exist!') 
except AuthenticationError:
    print('Account or password is incorrect!') 
except ExhaustedError:
    print('Quota used up!') 
except SDKError as e:
    print(f'SDK error: {e}')
```

arguments:

| arguments name | definition |
| --- | --- |
| env | the environment of SDK , kiri_convert.EnvType.Test is testing environment, kiri_convert.EnvType.Prod is production environment |
| account | account in our platform |
| password | password associates with the account |

Possible exception list:

| exception type | definition | solution |
| ----- | ----- | -----|
| AccountNotExistError | Account does not exist | Please register an account |
| AuthenticationError | Account or password is incorrect | Please check if your account or password is correct |
| ExhaustedError | Quota used up | Please contact us |
| SDKError | SDK initialize failed | Please contact us |

<br/>

#### 2.2 Converting

Code examples:

```python
# Converting
try:
    result = kiri_convert.convert(
        source_file='../models/StoneElephant.obj',
        texture_file='../models/StoneElephant.jpg',
        convert_format=kiri_convert.ModelFormat.FORMAT_XYZ,
        save_name='Export',
        save_path='../exports'
    )

    if result == kiri_convert.SUCCESS:
        print('Converting done!')
    else:
        print('Converting failed!')
except SDKError as e:
    print(f'SDK error: {e}')
```

arguments:

| arguments name | definition |
| --- | --- |
| source_file | OBJ input file path |
| texture_file | texture file path |
| convert_format | target converting file format should start with kiri_convert.ModelFormat.FORMAT_, following with the file format you want. For example, if you want xyz file format, it should be kiri_convert.ModelFormat.FORMAT_XYZ |
| save_name | output file name, no file extension |
| save_path | output file path |

⚠️ Note：OBJ、texture file、MTL file are better to store in the same root，otherwise the file path need to be corrected in obj and mtl files
