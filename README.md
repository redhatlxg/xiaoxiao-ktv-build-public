# 晓晓呀 KTV 临时构建入口

本仓库只保存公开的构建工作流，不复制私有应用源码。

## 安全设计

- Android 源码在运行时从私有仓库 `redhatlxg/xiaoxiao-ktv` 的 `feature/ui-commercial-karaoke-home-187` 分支临时检出。
- 私有源码不会提交到本公开仓库。
- 只构建电视端 `:app:assembleDebug`，不构建或发布手机授权签发器。
- 不读取、不上传签名证书或发布令牌；输出为 Android debug 签名测试 APK，不是正式签名包。
- 构建产物通过 Actions Artifact 提供下载。

## 首次设置（必须）

在本仓库添加 Actions Secret：`SOURCE_REPO_TOKEN`。

创建 GitHub Fine-grained personal access token：
1. Resource owner 选择 `redhatlxg`。
2. Repository access 仅选择 `xiaoxiao-ktv`。
3. Repository permissions 只给 `Contents: Read-only`（Metadata 为自动只读）。
4. 将生成的 token 添加到本仓库 Settings → Secrets and variables → Actions → New repository secret，名称必须为 `SOURCE_REPO_TOKEN`。

不要把 token 粘贴到 issue、代码、工作流文件或聊天中。

## 运行

设置 Secret 后，在本仓库 Actions 中运行 **Build private Android source (QA APK)** 工作流。

## 下载

成功后打开该次 Actions run，在 Artifacts 中下载 `xiaoxiao-ktv-ui-qa-apk`。

注意：该 APK 使用 Android debug 签名，仅用于测试安装；不能替代正式签名发布包。构建时会在临时 Runner 工作区把正式离线授权 HMAC 密钥替换成一次性随机 QA 密钥；公开测试 APK 不能验证正式授权码，也不包含可用于签发正式授权码的密钥。试用和界面测试可用，正式授权/商业分发请继续使用私有仓库中的正式签名发布流程。
