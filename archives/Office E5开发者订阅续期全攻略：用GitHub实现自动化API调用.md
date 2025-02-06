# Office E5开发者订阅续期全攻略：用GitHub实现自动化API调用

> 微软Office E5开发者订阅默认提供90天试用期，通过定期调用Microsoft Graph API可实现订阅续期。本文将以GitHub Actions为核心，详细讲解自动化续期方案的实施步骤。

**免责声明**：本文所述方法依赖微软官方API调用机制，实际续期成功率受微软审核策略影响，**不保证100%成功执行**（建议至少提前15天完成配置）。

---

## 一、环境准备要点

### 1. Microsoft Azure应用配置流程
- 使用E5管理员账号登录[Azure门户](https://portal.azure.com/#home)
- 通过左侧导航栏进入 **Azure Active Directory > 应用注册**
- 新应用配置建议：
    tex
    支持账户类型：任何组织目录
    重定向URI类型：Web
    重定向地址：http://localhost:53682/
    

### 2. API权限配置清单
必须勾选的6大类核心权限：

| 分类           | 读写权限                |
|----------------|------------------------|
| 文件系统       | Files.Read.All ｜ Files.ReadWrite.All |
| SharePoint     | Sites.Read.All ｜ Sites.ReadWrite.All |
| 用户信息       | User.Read.All ｜ User.ReadWrite.All   |
| 目录服务       | Directory.Read.All ｜ Directory.ReadWrite.All |
| 邮件服务       | Mail.Read ｜ Mail.ReadWrite |
| 邮箱设置       | MailboxSettings.Read ｜ MailboxSettings.ReadWrite |

**关键操作**：完成权限配置后需点击 **"代表XXX授予管理员同意"**

---

## 二、自动化配置完整教程

### 步骤1：Token生成操作
1. 下载[rclone工具包](https://downloads.rclone.org/v1.52.3/rclone-v1.52.3-windows-amd64.zip)
2. 在解压目录执行核心命令：
powershell
rclone authorize "onedrive" "your_client_id" "your_client_secret"

3. 复制生成的`refresh_token`值（注意清除引号）

👉 [野卡 | 一分钟注册，轻松订阅海外线上服务](https://bbtdd.com/yeka)

### 步骤2：GitHub项目配置
1. Fork开源项目`wangziyingwen/AutoApiSecret`到个人仓库
2. 配置关键参数：
    - 在`1.txt`中粘贴refresh_token
    - 新建Secrets：
        yml
        CONFIG_ID: id=r'你的应用ID'
        CONFIG_KEY: secret=r'你的应用密钥'
        
3. 生成Personal Access Token所需权限：
    tex
    repo / admin:repo_hook / workflow
    

---

## 三、运行状态监测
- 每日查看GitHub Actions执行日志
- 关注`Test Api`模块调用状态
- 建议定期更新项目代码库

---

## 操作视频参考
<iframe src="https://player.bilibili.com/player.html?aid=95688306&bvid=BV1mE411V74B&cid=163358877&page=1&high_quality=1&danmaku=0"></iframe>

👉 [需要开通海外服务？试试野卡虚拟信用卡](https://bbtdd.com/yeka)

---

**技术文档参考**：  
1. 基于GitHub Actions的自动化实现方案  
2. Microsoft Graph API官方说明文档  
3. Azure AD应用授权最佳实践指南