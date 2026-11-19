# Vue iOS 测试项目（Vue 3 + Vite + Capacitor）

## 本地开发（Windows 可用）

```bash
npm install
npm run dev          # 浏览器预览
npm run build        # 构建到 dist
npx cap sync ios     # 把 dist 同步进 iOS 工程
```

## 导出 IPA

IPA 只能在 macOS + Xcode 上编译，所以本项目用 GitHub Actions 的免费 macOS 机器打包：

1. **先把工作流文件放到位**：把 `ci\ios-ipa.yml` 移动到 `.github\workflows\ios-ipa.yml`（PowerShell：`mkdir .github\workflows; move ci\ios-ipa.yml .github\workflows\`）。
2. 在 GitHub 新建一个仓库（可设为私有）。
3. 在本目录执行：
   ```bash
   git init
   git add .
   git commit -m "init"
   git branch -M main
   git remote add origin https://github.com/<你的用户名>/<仓库名>.git
   git push -u origin main
   ```
4. 打开仓库的 **Actions** 页，等待「打包 iOS IPA」运行完成（约 5–10 分钟）。
5. 在运行详情页底部 **Artifacts** 下载 `VueIosDemo-ipa`，解压得到 `VueIosDemo-unsigned.ipa`。

## 安装到 iPhone

导出的是 **未签名 IPA**，不能直接安装，需要先签名：

- 用 **爱思助手 → IPA 签名**、**AltStore** 或 **Sideloadly**，登录你的 Apple ID 进行签名并安装（免费 Apple ID 签名有效期 7 天）。
- 如果有苹果开发者账号（$99/年），可以改造工作流，用证书 + 描述文件直接导出签名 IPA。

## 项目信息

- App 名称：VueIosDemo
- Bundle ID：com.example.vueiosdemo（可在 `capacitor.config.ts` 和 Xcode 工程中修改）
