*核心思路：让AI根据你的需求，自动找到合适的n8n工作流，然后参考他们来搭建你的工作流。

Step1：找到需求相关的json
让AI去搜索网上已有的工作流json，尤其是官方的模板库里面有6000多个模板，绝对有跟你需求接近的几个。

例如需求是：我准备搭建一个reddit监控的n8n工作流，用于品牌竞品、负面舆情、用户痛点洞察等需求。
向AI提问（chatgpt5 thinking）：
请你帮我到官方模板库（n8n.io/workflows/）、X推特、YouTube等地方查找最合适的、现成的n8n工作流模板给我参考，给我找10个，都要附上来源链接。

AI会返回一些链接。

Step2：一键批量下载json

让 claude code 调用 Playwright mcp 工具，逐个访问以下的8个N8N模板链接，在每个链接的页面，找到 use for free 按钮点击，在弹窗点击 Copy template to clipboard（JSON）然后在本地文件夹创建一个 json 文件把复制的内容黏贴进去。

于是就得到了多个跟你需求类似的 n8n 工作流 json 文件。

Step3：生成工作流

最后，还是在 Claude Code 里，参考以下提示词，让AI生成工作流文件即可。

提示词：
当前文件夹是和Reddit相关的n8n工作流json文件，你务必要每个文件都完整浏览一遍后，完成以下需求：

```  
大疆（DJI）Reddit舆情监控流程  
目标：自动监控Reddit上关于大疆及其竞品的讨论，及时发现问题和机会。

第一步：设定监控指令（Inputs）您需要提供两份清单：  
关键词列表（Keywords）：  
品牌词：DJI，大疆  
产品词：Mavic, Air, Mini, Inspire, Phantom, Avata, Osmo, Ronin  
痛点词：flyaway（炸机），GPS lost, battery drain, firmware update, no signal, customer service, gimbal issue, app crash, no-fly zone  
竞品词：Autel, Parrot, Skydio, Yuneec, Hubsan, PowerVision  

社区列表（Subreddits）：  
r/dji  
r/drones  
r/Multicopter  
r/UAV  
r/Quadcopter  
r/DronePhotography

第二步：N8N自动化流程（Workflow）  
定时启动：系统自动每周运行一次。  
抓取内容：自动抓取上述社区中，包含上述关键词的最新帖子和评论。  
结构化：对每个帖子/评论，标注分类和主题：  
主因：好评/负面/中性  
主题：飞行表现/硬件问题/软件/App/客户服务/售后服务/法规合规  
是否紧急：是/否  
自动处理：  
紧急情况（如严重负面）：立刻通过谷歌邮箱发送警报。  
所有情况：将分析结果自动写入一张Google表格中。

第三步：最终成果（Output）  
您会得到一个实时更新的在线报告，包含：  
话题趋势：过去7天热点及量，差评占比。  
品牌对比图：大疆 vs Autel vs Parrot 等每日讨论量。  
痛点排名图：用户抱怨最多的问题是什么（如炸机、固件问题等）。  
最新差评列表：包含原文链接，方便您快速处理。
```  
最终给我新建一个n8n工作流json文件，其中，注意AI相关任务通过AI Agent的节点搭配openai的model来完成。  

这样就基本实现了需要的功能。
