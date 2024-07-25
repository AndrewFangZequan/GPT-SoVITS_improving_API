# 项目介绍
本代码是针对 [**GPT-SoVITS**](https://github.com/RVC-Boss/GPT-SoVITS.git) 项目的补充，主要是为了解决原项目 **api** 接口功能不足的问题。

该 **api.py** 代码可以直接覆盖原 **GPT-SoVITS** 项目代码的 **api.py** 文件，并不会影响原项目的完整性。主要的改进如下：
* 提供在运行过程中切换模型（音色）的接口，支持 **POST** 和 **GET**
* 提供切换生成情绪的接口，即可以通过预设参考音频并赋予情绪的形式，在推理过程中快速选择指定情绪，支持 **POST** 和 **GET**
  
# 使用方法
### 执行参数(与原api一致)
-s - SoVITS模型路径, 可在 config.py 中指定  
-g - GPT模型路径, 可在 config.py 中指定

调用请求缺少参考音频时使用

-dr - 默认参考音频路径  
-dt - 默认参考音频文本  
-dl - 默认参考音频语种, "中文","英文","日文","zh","en","ja"  

-d - 推理设备, "cuda","cpu"  
-a - 绑定地址, 默认"127.0.0.1"  
-p - 绑定端口, 默认9880, 可在 config.py 中指定  
-fp - 覆盖 config.py 使用全精度  
-hp - 覆盖 config.py 使用半精度  
-sm - 流式返回模式, 默认不启用, "close","c", "normal","n", "keepalive","k"  
-mt - 返回的音频编码格式, 流式默认ogg, 非流式默认wav, "wav", "ogg", "aac"  
-cp - 文本切分符号设定, 默认为空, 以",.，。"字符串的方式传入  

-hb - cnhubert路径  
-b - bert路径  

---

### 推理调用
#### 切换推理模型：  
GET:  
    `http://127.0.0.1:9880/change_weights?sovits_weight_path=填SoVITS模型路径&gpt_weight_path=填GPT模型路径`  

POST:  
```
请求网址：http://127.0.0.1:9880/change_weights  
请求JSON：
{
    "gpt_weight_path":"填GPT模型路径",
    "sovits_weight_path":"填SoVITS模型路径"
}
```
返回ok即代表切换成功。

#### 切换各种情绪：
前期准备：准备一个命名为某某情绪的文件夹，文件夹中放入该情绪的参考音频，参考音频的名字改成对应的参考文字即可。注意每个文件夹内只能有1个参考音频文件，若有多个参考音频将自动选择第一个。目前仅支持中文参考音频。

GET:  
    `http://127.0.0.1:9880/emotion?emotions=情绪文件夹路径&text=要生成的文字（语言不限）&text_language=目标文字的语言&cut_punc=切分方式`    

POST:  
```
请求网址：http://127.0.0.1:9880/emotion
请求JSON：
{
    "emotions":"情绪文件夹路径",
    "text":"要生成的文字（语言不限）",
    "text_language":"目标文字的语言",
    "cut_punc":"切分方式"
}
```
其中，**cut_punc** 不强制要求，但是前三个参数必须传入。


