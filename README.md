# 授权工具 APK 编译说明

## 使用前必做：添加 GitHub Secrets

进入仓库 → Settings → Secrets and variables → Actions → New repository secret

需要添加以下 4 个 Secret：

| Secret 名           | 说明                        |
|--------------------|-----------------------------|
| `KEYSTORE_BASE64`  | keystore 文件的 base64 内容  |
| `KEYSTORE_PASSWORD`| keystore 密码               |
| `KEY_ALIAS`        | key 别名（建议填 release）   |
| `KEY_PASSWORD`     | key 密码                    |

---

## 生成 keystore（Windows PowerShell，只需做一次）

```powershell
# 生成 keystore（根据提示填写信息）
keytool -genkey -v `
  -keystore release.jks `
  -keyalg RSA -keysize 2048 -validity 10000 `
  -alias release `
  -storepass 你的密码 `
  -keypass 你的密码 `
  -dname "CN=YourName, O=YourOrg, C=CN"

# 转成 base64，复制输出内容填入 KEYSTORE_BASE64
[Convert]::ToBase64String([IO.File]::ReadAllBytes("release.jks")) | Out-File -NoNewline keystore.b64.txt
notepad keystore.b64.txt
```

---

## 触发编译

- 每次 push 到 main 分支自动触发
- 或者手动：Actions → Build Android APK → Run workflow

## 下载 APK

Actions → 对应的 workflow 运行记录 → 底部 Artifacts 下载

- `app-debug`：调试版，直接安装测试用
- `app-release`：签名发布版
