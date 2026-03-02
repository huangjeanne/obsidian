文档目的 ：本文档提供 ListenHub 音频生成服务的完整接入指南，包括快速开始、核心概念、API 使用和错误处理。

curl -X GET "https://api.marswave.ai/openapi/v1/speakers/list?language=zh"  \ -H "Authorization: Bearer YOUR\_API\_KEY"&#x20;



{  "code" :  0 ,  "message" :  "" ,  "data" :  {  "items" :  \[  {  "name" :  "原野" ,  "speakerId" :  "CN-Man-Beijing-V2" ,  "demoAudioUrl" :  "https://marswave-podcast-production.s3.us-west-2.amazonaws.com/audios/mars-CN-Man-Beijing-V2.mp3" ,  "gender" :  "male" ,  "language" :  "zh"  } ,  {  "name" :  "晓曼" ,  "speakerId" :  "chat-girl-105-cn" ,  "demoAudioUrl" :  "https://marswave-podcast-production.s3.us-west-2.amazonaws.com/audios/mars-chat-girl-105-cn.mp3" ,  "gender" :  "female" ,  "language" :  "zh"  }  ]  }  }&#x20;



curl -X POST "https://api.marswave.ai/openapi/v1/podcast/episodes"  \ -H "Authorization: Bearer YOUR\_API\_KEY"  \ -H "Content-Type: application/json"  \ -d '{ "query": "介绍人工智能的发展历史", "speakers": \[{"speakerId": "CN-Man-Beijing-V2"}], "language": "zh", "mode": "quick" }'&#x20;



{  "code" :  0 ,  "message" :  "" ,  "data" :  {  "episodeId" :  "68d699ebc4b373bd1ae50dde"  }  }&#x20;



curl -X GET "https://api.marswave.ai/openapi/v1/podcast/episodes/68d699ebc4b373bd1ae50dde"  \ -H "Authorization: Bearer YOUR\_API\_KEY"&#x20;



{  "code" :  0 ,  "message" :  "" ,  "data" :  {  "episodeId" :  "68ef5d2b5d59db3b0909c08c" ,  "createdAt" :  1760517419993 ,  "failCode" :  0 ,  "processStatus" :  "pending" ,  "credits" :  0 ,  "sourceProcessResult" :  {  "content" :  "介绍人工智能的发展历史\n\n" ,  "references" :  \[ ]  } ,  "title" :  "人工智能简史:机器如何学会思考" ,  "outline" :  "" ,  "cover" :  "https://static.listenhub.ai/listenhub\_default\_cover061802.png" ,  "audioUrl" :  "" ,  "scripts" :  \[ ]  }  }&#x20;



{  "code" :  0 ,  "message" :  "" ,  "data" :  {  "episodeId" :  "68ef5e78e06b4dd24fbfe69f" ,  "createdAt" :  1760517752411 ,  "failCode" :  0 ,  "processStatus" :  "success" ,  "credits" :  27 ,  "sourceProcessResult" :  {  "content" :  "介绍人工智能的发展历史\n\n" ,  "references" :  \[ ]  } ,  "title" :  "听AI讲自己的故事：智能时代如何诞生" ,  "outline" :  "..." ,  "cover" :  "https://static.listenhub.ai/listenhub\_default\_cover061804.png" ,  "audioUrl" :  "https://assets.listenhub.ai/listenhub-public-prod/podcast/68ef5e78e06b4dd24fbfe69d.mp3" ,  "scripts" :  \[  {  "speakerId" :  "chinese-mandarin-radio-host" ,  "speakerName" :  "宇航" ,  "content" :  "如今我们好像每天都被人工智能的新闻包围着。"  }  ]  }  }&#x20;



import time import requests def  poll\_episode\_result ( episode\_id , api\_key , timeout = 300 ) :  """ 轮询查询单集生成结果 Args: episode\_id: 单集ID api\_key: API密钥 timeout: 超时时间（秒），默认5分钟 """ url =  f"https://api.marswave.ai/openapi/v1/podcast/episodes/ { episode\_id } " headers =  { "Authorization" :  f"Bearer { api\_key } " } start\_time = time . time ( )  print ( "⏳ 等待内容生成中..." ) time . sleep ( 60 )   while time . time ( )  - start\_time < timeout : response = requests . get ( url , headers = headers ) data = response . json ( )  if data \[ "code" ]  !=  0 :  raise Exception ( f"API Error: { data \[ 'message' ] } " ) status = data \[ "data" ] \[ "processStatus" ]  if status ==  "success" :  return data \[ "data" ]  elif status ==  "failed" :  raise Exception ( f"Generation failed: { data \[ 'data' ] . get ( 'message' ) } " ) time . sleep ( 10 )   raise TimeoutError ( "Episode generation timeout" )&#x20;



Episode（单集） ：ListenHub 系统的基本内容单元

获取方式 ：调用

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">GET /v1/speakers/list</span>

接口获取可用音色列表

重要说明 ： Podcast 模式 ：支持选择 1-2 个声音/speaker（单人或双人播客）

文本流（Server-Sent Events 格式）

Podcast ：大纲和脚本数据，创建后 20–60 秒可用

FlowSpeech ：大纲和脚本数据，创建后约 3 秒可用

流式音频（M3U8） ：适合实时播放，字段名

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">audioStreamUrl</span>

完整音频（MP3） ：适合下载和离线播放，字段名

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">audioUrl</span>

ListenHub Playground 提供了在线体验多音色语音合成的功能，无需编写代码即可快速测试。

Pro、Max 或 Enterprise 会员可创建 API Key

Authorization: Bearer < your\_api\_key >&#x20;



Base URL: https://api.marswave.ai



测试环境 ： 如需接入测试环境，请联系 ListenHub 技术团队 support@marswave.ai

说明 ：获取所有可用的音色，用于后续创建内容时选择音色。

&#x20;curl -X GET "https://api.marswave.ai/openapi/v1/speakers/list?language=zh"  \ -H "Authorization: Bearer YOUR\_API\_KEY"   curl -X GET "https://api.marswave.ai/openapi/v1/speakers/list?language=en"  \ -H "Authorization: Bearer YOUR\_API\_KEY"&#x20;



{  "code" :  0 ,  "message" :  "" ,  "data" :  {  "items" :  \[  {  "name" :  "原野" ,  "speakerId" :  "CN-Man-Beijing-V2" ,  "demoAudioUrl" :  "https://example.com/demo-male.mp3" ,  "gender" :  "male" ,  "language" :  "zh"  } ,  {  "name" :  "晓曼" ,  "speakerId" :  "chat-girl-105-cn" ,  "demoAudioUrl" :  "https://example.com/demo-female.mp3" ,  "gender" :  "female" ,  "language" :  "zh"  }  ]  }  }&#x20;



说明 ：创建一个单主持人播客。支持三种模式：

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">deep</span>

（深度）、

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">quick</span>

（快速）、

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">debate</span>

（辩论，需要2个音色）。

curl -X POST "https://api.marswave.ai/openapi/v1/podcast/episodes"  \ -H "Authorization: Bearer YOUR\_API\_KEY"  \ -H "Content-Type: application/json"  \ -d '{ "query": "深度解析大语言模型的技术原理和应用前景", "speakers": \[ {"speakerId": "CN-Man-Beijing-V2"} ], "language": "zh", "mode": "deep" }'&#x20;



curl -X POST "https://api.marswave.ai/openapi/v1/podcast/episodes"  \ -H "Authorization: Bearer YOUR\_API\_KEY"  \ -H "Content-Type: application/json"  \ -d '{ "query": "今日科技新闻速报", "speakers": \[ {"speakerId": "chat-girl-105-cn"} ], "language": "zh", "mode": "quick" }'&#x20;



import requests def  create\_solo\_podcast ( query , speaker\_id , mode = "quick" ) :  """ 创建单人播客 Args: query: 播客主题 speaker\_id: 音色ID（从音色列表接口获取） mode: 生成模式 (deep/quick/debate) """ url =  "https://api.marswave.ai/openapi/v1/podcast/episodes" headers =  {  "Authorization" :  f"Bearer { API\_KEY } " ,  "Content-Type" :  "application/json"  } data =  {  "query" : query ,  "speakers" :  \[ { "speakerId" : speaker\_id } ] ,  "language" :  "zh" ,  "mode" : mode } response = requests . post ( url , headers = headers , json = data )  return response . json ( )  result = create\_solo\_podcast ( query = "人工智能的发展历史" , speaker\_id = "CN-Man-Beijing-V2" , mode = "quick"  )  print ( f"Episode ID: { result \[ 'data' ] \[ 'episodeId' ] } " )&#x20;



说明 ：创建双主持人对话式播客，支持

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">deep</span>

、

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">quick</span>

、

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">debate</span>

三种模式。

curl -X POST "https://api.marswave.ai/openapi/v1/podcast/episodes"  \ -H "Authorization: Bearer YOUR\_API\_KEY"  \ -H "Content-Type: application/json"  \ -d '{ "query": "探讨人工智能对就业市场的影响", "speakers": \[ {"speakerId": "CN-Man-Beijing-V2"}, {"speakerId": "chat-girl-105-cn"} ], "language": "zh", "mode": "deep" }'&#x20;



&#x20;curl -X POST "https://api.marswave.ai/openapi/v1/podcast/episodes"  \ -H "Authorization: Bearer YOUR\_API\_KEY"  \ -H "Content-Type: application/json"  \ -d '{ "query": "远程办公是否应该成为主流工作模式？", "speakers": \[ {"speakerId": "CN-Man-Beijing-V2"}, {"speakerId": "chat-girl-105-cn"} ], "language": "zh", "mode": "debate" }'&#x20;



curl -X POST "https://api.marswave.ai/openapi/v1/podcast/episodes"  \ -H "Authorization: Bearer YOUR\_API\_KEY"  \ -H "Content-Type: application/json"  \ -d '{ "query": "分析这篇技术文章的核心观点", "sources": \[ { "type": "url", "content": "https://blog.samaltman.com/reflections" } ], "speakers": \[ {"speakerId": "CN-Man-Beijing-V2"}, {"speakerId": "chat-girl-105-cn"} ], "language": "zh", "mode": "deep" }'&#x20;



import requests def  create\_dual\_podcast ( query , speaker\_ids , mode = "quick" ) :  """ 创建双人播客 Args: query: 播客主题 speaker\_ids: 音色ID列表（2个） mode: 播客模式（quick/deep/debate） """ url =  "https://api.marswave.ai/openapi/v1/podcast/episodes" headers =  {  "Authorization" :  f"Bearer { API\_KEY } " ,  "Content-Type" :  "application/json"  } data =  {  "query" : query ,  "speakers" :  \[ { "speakerId" : sid }  for sid in speaker\_ids ] ,  "language" :  "zh" ,  "mode" : mode } response = requests . post ( url , headers = headers , json = data )  return response . json ( )  result = create\_dual\_podcast (  "讨论人工智能的伦理问题" ,  \[ "CN-Man-Beijing-V2" ,  "chat-girl-105-cn" ] ,  "debate"  )  print ( "Episode ID:" , result \[ "data" ] \[ "episodeId" ] )&#x20;



说明 ：将播客生成分为两个独立阶段，适用于需要审核或修改脚本的场景。

⚠️ language 参数（必填） ： ⚠️ language 参数（必填） ：

✅ Speaker 的语言必须与 language 参数一致

❌ 如果不一致，将返回错误：

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">Speaker language mismatch: expected {language}, but got speakers: {speakerId}({actualLanguage}))</span>

{  "language" :  "zh" ,  "speakers" :  \[ { "speakerId" :  "CN-Man-Beijing-V2" } ]   }&#x20;



{  "language" :  "en" ,  "speakers" :  \[ { "speakerId" :  "CN-Man-Beijing-V2" } ]   }&#x20;



curl -X POST "https://api.marswave.ai/openapi/v1/podcast/episodes/text-content"  \ -H "Authorization: Bearer YOUR\_API\_KEY"  \ -H "Content-Type: application/json"  \ -d '{ "query": "深入探讨量子计算的发展现状", "speakers": \[ {"speakerId": "CN-Man-Beijing-V2"}, {"speakerId": "chat-girl-105-cn"} ], "language": "zh", "mode": "deep" }'&#x20;



{  "code" :  0 ,  "message" :  "" ,  "data" :  {  "episodeId" :  "68d699ebc4b373bd1ae50dde" ,  "message" :  "Text content generation started. Audio generation can be triggered later."  }  }&#x20;



curl -X GET "https://api.marswave.ai/openapi/v1/podcast/episodes/68d699ebc4b373bd1ae50dde"  \ -H "Authorization: Bearer YOUR\_API\_KEY"&#x20;



⚠️ 重要 ：判断文本生成完成的依据是

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">contentStatus</span>

为

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">text-success</span>

{  "code" :  0 ,  "message" :  "" ,  "data" :  {  "episodeId" :  "68d699ebc4b373bd1ae50dde" ,  "processStatus" :  "success" ,  "contentStatus" :  "text-success" ,   "credits" :  15 ,  "title" :  "量子计算的现在与未来" ,  "outline" :  "..." ,  "scripts" :  \[  {  "speakerId" :  "CN-Man-Beijing-V2" ,  "speakerName" :  "原野" ,  "content" :  "大家好，今天我们来聊聊量子计算..."  } ,  {  "speakerId" :  "chat-girl-105-cn" ,  "speakerName" :  "晓曼" ,  "content" :  "是的，量子计算是一个令人激动的领域..."  }  ]  }  }&#x20;



curl -X POST "https://api.marswave.ai/openapi/v1/podcast/episodes/68d699ebc4b373bd1ae50dde/audio"  \ -H "Authorization: Bearer YOUR\_API\_KEY"  \ -H "Content-Type: application/json"&#x20;



curl -X POST "https://api.marswave.ai/openapi/v1/podcast/episodes/68d699ebc4b373bd1ae50dde/audio"  \ -H "Authorization: Bearer YOUR\_API\_KEY"  \ -H "Content-Type: application/json"  \ -d '{ "scripts": \[ { "content": "大家好，欢迎收听本期播客，今天我们深入探讨量子计算...", "speakerId": "CN-Man-Beijing-V2" }, { "content": "量子计算确实是一个革命性的技术...", "speakerId": "chat-girl-105-cn" } ] }'&#x20;



{  "code" :  0 ,  "message" :  "" ,  "data" :  {  "success" :  true ,  "message" :  "Audio generation started" ,  "episodeId" :  "68d699ebc4b373bd1ae50dde" ,  "status" :  "pending"  }  }&#x20;



curl -X GET "https://api.marswave.ai/openapi/v1/podcast/episodes/68d699ebc4b373bd1ae50dde"  \ -H "Authorization: Bearer YOUR\_API\_KEY"&#x20;



⚠️ 重要 ：判断音频生成完成的依据是

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">contentStatus</span>

为

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">audio-success</span>

{  "code" :  0 ,  "message" :  "" ,  "data" :  {  "episodeId" :  "68d699ebc4b373bd1ae50dde" ,  "processStatus" :  "success" ,  "contentStatus" :  "audio-success" ,   "credits" :  42 ,  "title" :  "量子计算的现在与未来" ,  "audioUrl" :  "https://assets.listenhub.ai/podcast/68d699ebc4b373bd1ae50dde.mp3" ,  "audioStreamUrl" :  "https://assets.listenhub.ai/podcast/68d699ebc4b373bd1ae50dde.m3u8" ,  "scripts" :  \[ ... ]  }  }&#x20;



import requests import time def  create\_podcast\_with\_review ( query , speaker\_ids , api\_key , mode = "deep" ) :  """ 两阶段播客生成：先生成脚本，审核后再生成音频 Args: query: 播客主题 speaker\_ids: 音色ID列表 api\_key: API密钥 mode: 生成模式 """ base\_url =  "https://api.marswave.ai/openapi/v1/podcast/episodes" headers =  {  "Authorization" :  f"Bearer { api\_key } " ,  "Content-Type" :  "application/json"  }   print ( "📝 第一阶段：生成文本内容..." ) text\_data =  {  "query" : query ,  "speakers" :  \[ { "speakerId" : sid }  for sid in speaker\_ids ] ,  "language" :  "zh" ,  "mode" : mode } response = requests . post ( f" { base\_url } /text-content" , headers = headers , json = text\_data ) episode\_id = response . json ( ) \[ "data" ] \[ "episodeId" ]  print ( f"✅ 已创建任务: { episode\_id } " )   print ( "⏳ 等待文本生成..." ) time . sleep ( 60 )   while  True : result = requests . get ( f" { base\_url } / { episode\_id } " , headers = headers ) . json ( ) content\_status = result \[ "data" ] . get ( "contentStatus" )   if content\_status ==  "text-success" :  print ( "✅ 文本内容生成完成" ) scripts = result \[ "data" ] \[ "scripts" ]  print ( f"📄 共生成 { len ( scripts ) } 段脚本" )  break  elif content\_status ==  "text-fail" :  raise Exception ( "❌ 文本生成失败" ) time . sleep ( 10 )   modified\_scripts = scripts   print ( "🎵 第二阶段：生成音频..." ) audio\_data =  {  "scripts" :  \[  { "content" : s \[ "content" ] ,  "speakerId" : s \[ "speakerId" ] }  for s in modified\_scripts ]  } response = requests . post (  f" { base\_url } / { episode\_id } /audio" , headers = headers , json = audio\_data )  print ( "✅ 音频生成已启动" )   print ( "⏳ 等待音频生成..." ) time . sleep ( 60 )  while  True : result = requests . get ( f" { base\_url } / { episode\_id } " , headers = headers ) . json ( ) content\_status = result \[ "data" ] . get ( "contentStatus" )   if content\_status ==  "audio-success" :  print ( "✅ 音频生成完成" )  return result \[ "data" ]  elif content\_status ==  "audio-fail" :  raise Exception ( "❌ 音频生成失败" ) time . sleep ( 10 )  result = create\_podcast\_with\_review ( query = "探讨人工智能的伦理挑战" , speaker\_ids = \[ "CN-Man-Beijing-V2" ,  "chat-girl-105-cn" ] , api\_key = "YOUR\_API\_KEY" , mode = "deep"  )  print ( f"🎉 播客生成完成" )  print ( f"标题: { result \[ 'title' ] } " )  print ( f"音频: { result \[ 'audioUrl' ] } " )  print ( f"总积分: { result \[ 'credits' ] } " )&#x20;



轮询时 必须检查

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">contentStatus</span>

字段 ，而非

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">processStatus</span>

只有当

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">contentStatus == "text-success"</span>

时才能调用音频生成接口

使用

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">processStatus</span>

判断可能导致状态错误

说明 ：将文本直接转换为语音。支持两种模式：

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">smart</span>

（智能优化）和

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">direct</span>

（文本直接转换语音）。 单次生成内容至少需要大于 10 个字符。

curl -X POST "https://api.marswave.ai/openapi/v1/flow-speech/episodes"  \ -H "Authorization: Bearer YOUR\_API\_KEY"  \ -H "Content-Type: application/json"  \ -d '{ "sources": \[ { "type": "text", "content": "欢迎使用ListenHub音频生成服务这是一段测试文本会自动修复错别字和标点符号" } ], "speakers": \[ {"speakerId": "chat-girl-105-cn"} ], "language": "zh", "mode": "smart" }'&#x20;



curl -X POST "https://api.marswave.ai/openapi/v1/flow-speech/episodes"  \ -H "Authorization: Bearer YOUR\_API\_KEY"  \ -H "Content-Type: application/json"  \ -d '{ "sources": \[ { "type": "text", "content": "欢迎使用 ListenHub 音频生成服务。这是一段已经完善的文本，将直接转换为语音。" } ], "speakers": \[ {"speakerId": "CN-Man-Beijing-V2"} ], "language": "zh", "mode": "direct" }'&#x20;



<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">smart</span>

：AI 智能模式，自动修复语句不通顺、错别字、标点符号等问题

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">direct</span>

：直接模式，不修改用户内容，直接转换为语音

curl -X POST "https://api.marswave.ai/openapi/v1/flow-speech/episodes"  \ -H "Authorization: Bearer YOUR\_API\_KEY"  \ -H "Content-Type: application/json"  \ -d '{ "sources": \[ { "type": "url", "content": "https://example.com/article.html" } ], "speakers": \[ {"speakerId": "chat-girl-105-cn"} ], "language": "zh", "mode": "smart" }'&#x20;



def  create\_flowspeech ( text , speaker\_id , mode = "smart" ) :  """ 创建 FlowSpeech 语音 Args: text: 要转换的文本 speaker\_id: 音色ID（从音色列表接口获取） mode: smart（AI优化）或 direct（文本直接转换） """ url =  "https://api.marswave.ai/openapi/v1/flow-speech/episodes" headers =  {  "Authorization" :  f"Bearer { API\_KEY } " ,  "Content-Type" :  "application/json"  } data =  {  "sources" :  \[ { "type" :  "text" ,  "content" : text } ] ,  "speakers" :  \[ { "speakerId" : speaker\_id } ] ,  "language" :  "zh" ,  "mode" : mode } response = requests . post ( url , headers = headers , json = data )  return response . json ( )  result = create\_flowspeech ( text = "这是需要AI优化的文本内容" , speaker\_id = "chat-girl-105-cn" , mode = "smart"  )&#x20;



curl -X POST "https://api.marswave.ai/openapi/v1/speech"  \ -H "Authorization: Bearer YOUR\_API\_KEY"  \ -H "Content-Type: application/json"  \ -d '{ "scripts": \[ { "content": "大家好，欢迎收听今天的节目", "speakerId": "CN-Man-Beijing-V2" }, { "content": "今天我们要聊一个有趣的话题", "speakerId": "chat-girl-105-cn" }, { "content": "是的，让我们开始吧", "speakerId": "CN-Man-Beijing-V2" } ] }'&#x20;



def  create\_speech ( scripts , api\_key ) :  """ 使用脚本直接生成语音（同步） Args: scripts: 脚本列表，每个脚本包含 content 和 speakerId api\_key: API密钥 Returns: 包含 audioUrl、audioDuration、subtitlesUrl、taskId 和 credits 的响应 """ url =  "https://api.marswave.ai/openapi/v1/speech" headers =  {  "Authorization" :  f"Bearer { api\_key } " ,  "Content-Type" :  "application/json"  } data =  { "scripts" : scripts } response = requests . post ( url , headers = headers , json = data )  return response . json ( )  scripts =  \[  { "content" :  "欢迎大家" ,  "speakerId" :  "CN-Man-Beijing-V2" } ,  { "content" :  "今天的主题是..." ,  "speakerId" :  "chat-girl-105-cn" } ,  ] result = create\_speech ( scripts ,  "YOUR\_API\_KEY" )  print ( f"音频 URL: { result \[ 'data' ] \[ 'audioUrl' ] } " )  print ( f"字幕 URL: { result \[ 'data' ] \[ 'subtitlesUrl' ] } " )  print ( f"时长: { result \[ 'data' ] \[ 'audioDuration' ] } ms" )  print ( f"积分: { result \[ 'data' ] \[ 'credits' ] } " )&#x20;



curl -X GET "https://api.marswave.ai/openapi/v1/user/subscription"  \ -H "Authorization: Bearer YOUR\_API\_KEY"&#x20;



{  "code" :  0 ,  "message" :  "" ,  "data" :  {  "subscriptionStartedAt" :  1735660800000 ,  "subscriptionExpiresAt" :  1767196800000 ,  "usageAvailableMonthlyCredits" :  500 ,  "usageTotalMonthlyCredits" :  1000 ,  "usageAvailablePermanentCredits" :  300 ,  "usageTotalPermanentCredits" :  500 ,  "usageAvailableLimitedTimeCredits" :  200 ,  "totalAvailableCredits" :  1000 ,  "resetAt" :  1735660800000 ,  "platform" :  "web" ,  "renewStatus" :  false ,  "paidStatus" :  true ,  "subscriptionPlan" :  {  "name" :  "pro" ,  "duration" :  "monthly" ,  "platform" :  "web"  }  }  }&#x20;



<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">totalAvailableCredits</span>

：总可用积分（重要）

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">usageAvailableMonthlyCredits</span>

：本月剩余积分

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">usageAvailablePermanentCredits</span>

：永久积分余额

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">usageAvailableLimitedTimeCredits</span>

：限时积分余额

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">subscriptionExpiresAt</span>

：订阅到期时间

说明： 所有 API 响应都使用 HTTP 200 状态码，通过响应体中的

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">code</span>

字段区分成功与失败：

未列举的其他非0错误码：表示其他错误，请查看

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">message</span>

字段或联系技术支持

Q: 如何获取 API Key？ A: 访问 [API Keys 设置页面 ](https://listenhub.ai/zh/settings/api-keys)，需订阅 Pro、Max 或 Enterprise 套餐。

Q: 为什么返回 21007 错误？ A: API Key 无效。请检查：

Authorization 头格式是否为：

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">Bearer API_KEY</span>

Q: 如何判断任务是否完成？ A: 轮询查询接口，当

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">processStatus</span>

为

<span style="color: rgb(216,57,49); background-color: rgb(242,243,245)">success</span>

时表示完成。

RPM ：Requests Per Minute，每分钟请求次数

SSE ：Server-Sent Events，服务器推送事件

Speaker ：音频生成的核心参数，定义了单集的声学特性。每个 Speaker 包含以下关键属性：

如有任何问题，请随时联系我们：support@marswave.ai
