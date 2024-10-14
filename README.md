# Azure TTS PopClip插件

## 描述

这是一个为PopClip设计的Azure文本转语音(TTS)插件。它允许用户选择文本,并使用Azure的TTS服务将其转换为语音。这个插件提供了多项可自定义选项,包括Azure区域、订阅密钥、语音速度和语音名称。

## 功能

- 将选中的文本转换为语音
- 可自定义Azure区域和订阅密钥
- 可调整语音速度
- 可选择不同的语音
- 默认使用中文语音(zh-CN-XiaoxiaoMultilingualNeural)

## 安装

1. 确保您已安装PopClip
2. 下载`tts`脚本文件
3. 将`tts`文件拖放到PopClip的扩展配置窗口中



## 配置

在PopClip的设置中,您需要配置以下选项:

1. **Azure Region**: 您的Azure TTS服务所在的区域
2. **Azure Subscription Key**: 您的Azure TTS服务订阅密钥
3. **Speech Rate**: 语音速度,例如"+20%"表示加快20%,"-10%"表示减慢10%
4. **Voice Name**: 语音名称,默认为"zh-CN-XiaoxiaoMultilingualNeural"

## 使用方法





```
#!/bin/zsh
# #popclip
# name: Azure TTS
# icon: symbol:message.and.waveform
# options:
# - {type: string, identifier: AZURE_REGION, label: Azure Region, default: }
# - {type: string, identifier: AZURE_SUBSCRIPTION_KEY, label: Azure Subscription Key, default: }
# - {type: string, identifier: SPEECH_RATE, label: Speech Rate (e.g. +20% or -10%), default: +0%}
# - {type: string, identifier: VOICE_NAME, label: Voice Name, default: zh-CN-XiaoxiaoMultilingualNeural}

# 从PopClip选项中获取配置
AZURE_REGION="$POPCLIP_OPTION_AZURE_REGION"
AZURE_SUBSCRIPTION_KEY="$POPCLIP_OPTION_AZURE_SUBSCRIPTION_KEY"
SPEECH_RATE="$POPCLIP_OPTION_SPEECH_RATE"
VOICE_NAME="$POPCLIP_OPTION_VOICE_NAME"

# 创建临时音频文件
temp_audio_file=$(mktemp)

# 使用curl下载并保存音频数据到临时文件
curl -X POST "https://${AZURE_REGION}.tts.speech.microsoft.com/cognitiveservices/v1" \
     -H "Ocp-Apim-Subscription-Key: ${AZURE_SUBSCRIPTION_KEY}" \
     -H "Content-Type: application/ssml+xml" \
     -H "X-Microsoft-OutputFormat: audio-16khz-32kbitrate-mono-mp3" \
     -d "<speak version=\"1.0\" xmlns=\"http://www.w3.org/2001/10/synthesis\" xml:lang=\"zh-CN\">
    <voice name=\"${VOICE_NAME}\">
        <prosody rate=\"${SPEECH_RATE}\">
            $POPCLIP_TEXT
        </prosody>
    </voice>
</speak>" -so "$temp_audio_file"

# 使用afplay播放临时音频文件
afplay "$temp_audio_file"

# 清理临时音频文件
rm "$temp_audio_file"
```
- 选中以上代码，在弹出的popclip界面中点击安装，如下图所示
![install](https://res.craft.do/user/full/f3be568a-34ab-2b61-a799-bb5c9d6f8f77/doc/B888CE2A-C978-4EBE-B50C-175C38CD0188/403586BF-5455-49E1-9D5C-E37D4325174B_2/x6Nd7nWq31omS9Tx7faGoBoCfz5UA4TuuCxe4sBWDnAz/Image.png)
- 选择要转换为语音的文本
- 点击PopClip菜单中的Azure TTS图标
- 插件将使用Azure TTS服务将文本转换为语音并播放

## 注意事项

- 请确保您有有效的Azure TTS服务订阅
- 如果选择非中文语音,可能需要手动调整脚本中的语言设置
- 使用不同的语音可能需要更改Azure区域设置

## 故障排除

如果插件无法正常工作,请检查:

1. Azure区域和订阅密钥是否正确
2. 网络连接是否正常
3. 选择的语音是否在您的Azure订阅中可用


