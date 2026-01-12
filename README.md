🧠 Cognitive Assessment System (v5.6)
临床认知评估系统 (Clinical Research Edition)

这是一个轻量级、基于 Web 的神经心理学认知评估工具。旨在为临床医生和科研人员提供一种便携、低成本且标准化的方式，用于筛查轻度认知障碍 (MCI) 及阿尔茨海默病 (AD) 风险。

✨ 核心特性 (Features)
零依赖 (Zero Dependency)：单文件 HTML 架构，下载即用，无需安装 App 或配置服务器。

跨平台兼容：完美适配 iPad、手机及桌面浏览器（针对 Safari 和触屏优化）。

多语言支持：内置 简体中文、English 及 儿童版 (Child Mode) 界面。

临床级指标：

自动计算 记忆保持率 (Retention Rate)：区分海马体存储障碍与提取障碍。

监测 再认虚报率 (False Alarms)：评估源记忆与执行监控功能。

记录 反应时变异性 (Response Variability)：捕捉早期注意网络受损。

AI & 云端集成：

支持 Gemini Pro API：对测试结果进行即时、专业的临床解读。

支持 Google Sheets：一键将数据同步至云端表格。

📋 评估流程 (Protocol)
本测试全流程约耗时 10-15 分钟，包含 6 个核心范式：

T1 学习与即时回忆 (Immediate Recall)

任务：听/看 10 个词语并立即回忆。

评估：编码能力与短时记忆容量。

T2 空间记忆 (Spatial Memory / Corsi Block)

任务：复现方块亮起的顺序（自适应难度）。

评估：视空间工作记忆（右半球功能）。

T3 定向力 (Orientation)

任务：回答时间与地点（自动校验时间）。

评估：作为干扰任务，同时评估基础认知状态。

T4 延迟回忆 (Delayed Recall)

任务：再次回忆 T1 学过的词语。

评估：海马体长时记忆巩固功能（AD 核心指标）。

T5 再认 (Recognition)

任务：从混杂词表中辨认目标词。

评估：区分存储障碍（AD特征）与提取障碍（抑郁/血管性特征）。

T6 处理速度 (SDMT Speed)

任务：符号-数字快速匹配。

评估：信息处理速度、注意力稳定性及额叶执行功能。

🚀 快速开始 (Quick Start)
方式一：直接使用
下载本项目中的 index.html 文件。

双击在 Chrome、Safari 或 Edge 浏览器中打开。

开始测试。

方式二：部署到 GitHub Pages
Fork 本仓库。

在仓库的 Settings -> Pages 中，将 Source 设置为 main branch。

获取在线链接，发送给受试者即可远程测试。

⚙️ 高级配置 (Configuration)
点击首页右上角的 "🔧 设置 / Settings" 按钮，可配置以下高级功能：

1. ☁️ 配置云端存储 (Google Sheets)
若需将数据保存到 Google 表格，请按以下步骤操作：

创建一个新的 Google Sheet。

点击 扩展程序 (Extensions) > Apps Script。

复制并粘贴下方的 Google Apps Script 代码。

点击 部署 (Deploy) > 新建部署 (New Deployment) > 选择 Web 应用 (Web App)。

重要：将“谁可以访问 (Who has access)”设置为 "任何人 (Anyone)"。

复制生成的 Web App URL，填入测试系统的设置框中。

<details> <summary>点击查看 Google Apps Script 代码</summary>

JavaScript

function doPost(e) {
  var lock = LockService.getScriptLock();
  lock.tryLock(10000);
  
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var data = JSON.parse(e.postData.contents);
    
    // 展平数据结构 (根据实际需求调整)
    var rowData = [
      new Date(),
      data.patient.id,
      data.patient.name,
      data.patient.age,
      data.patient.gender,
      data.patient.edu,
      // Scores
      data.t1.entered.length, // T1 Score
      data.t2.score,          // T2 Score
      data.t3.score,          // T3 Score
      data.t4.entered.length, // T4 Score
      data.t5.hits,           // T5 Hits
      data.t5.false_alarms,   // T5 FA
      data.t6.score,          // T6 Score
      // Raw Data String
      JSON.stringify(data)
    ];
    
    sheet.appendRow(rowData);
    return ContentService.createTextOutput(JSON.stringify({"result":"success", "row": rowData}))
      .setMimeType(ContentService.MimeType.JSON);
      
  } catch (e) {
    return ContentService.createTextOutput(JSON.stringify({"result":"error", "error": e}))
      .setMimeType(ContentService.MimeType.JSON);
  } finally {
    lock.releaseLock();
  }
}
</details>

2. 🤖 配置 AI 专家解读
前往 Google AI Studio 获取免费的 API Key。

将 Key 填入测试系统的设置框中。

测试结束后点击“AI 专家解读”，即可获得基于分数的临床建议。

注意：API Key 仅保存在您浏览器的本地缓存（LocalStorage）中，不会上传至 GitHub。

🔒 隐私说明 (Privacy)
本系统默认运行在客户端（浏览器）本地。

除非您手动点击“上传云端”或“AI 解读”，否则没有任何数据会离开您的设备。

建议在使用云端功能前，对受试者姓名进行脱敏处理（使用 ID 代替）。

📄 License
This project is licensed under the MIT License - see the LICENSE file for details.
