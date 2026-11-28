# 贝勒府进销存 · GitHub Pages 部署说明

单文件网页应用，不依赖服务器。放到 GitHub Pages 后得到固定网址，手机/电脑都能打开，数据走免费云端数据库。

## 一、上传（不用装 Git，网页就能传）

1. 打开 https://github.com/new
2. Repository name 填：`belefu-inventory`
3. 选 **Public**（私有仓库的 Pages 需要付费，别选 Private）
4. 勾选 **Add a README file** → 点 Create repository
5. 进入仓库 → 点 **Add file** → **Upload files**
6. 把本文件夹里的 `index.html` 和 `.nojekyll` 两个文件拖进去（README 仓库里已有，会被覆盖成这份说明，没关系）
   - 注意：`.nojekyll` 是隐藏文件，Windows 资源管理器里要勾「显示隐藏文件」才能看到；拖不进去也问题不大，本应用没有下划线开头的路径
7. 底部点 **Commit changes**

## 二、开启网页访问

1. 仓库页 → **Settings** → 左侧 **Pages**
2. Source 选 **Deploy from a branch**
3. Branch 选 `main`，目录选 `/ (root)` → **Save**
4. 等 1～3 分钟，页面顶部会出现网址，形如：
   `https://你的用户名.github.io/belefu-inventory/`
5. 这个地址就是你的进销存系统，手机浏览器直接打开即可

## 三、更新版本

以后我改了 `index.html`，你只要：仓库 → Add file → Upload files → 覆盖 `index.html` → Commit。
约 1 分钟后线上就更新（浏览器按 Ctrl+F5 或下拉刷新）。

## 四、手机和电脑共用一份数据（已内置 Supabase，开箱即用）

这一版已经把免费数据库地址和 key 写进代码：**打开页面就自动连上云端，什么都不用填。**

- 数据库：Supabase 免费版（Postgres），项目 `wfqaqdbkpkcrrpexwptw`，房间码 `blf-7k29xq4m`
- 手机、电脑打开同一个网址 → 看到的就是同一份库存
- 改完数据约 8 秒自动上传；另一台打开页面会自动拉取最新
- 只有账面数据（商品/库存/单据/账号）走云端，条码、图片都在本机生成

第一次上手：

1. 电脑打开页面 →「系统设置」→「手机电脑同步（云端）」应显示 **云端已连接（Supabase）**
2. 点 **上传到云端**（把现有库存先推上去）
3. 手机打开同一网址 → 设置页同样显示已连接 → 点 **从云端下载** → 登录你自己建的账号
4. 之后两台随便改，自动同步

注意：
- 覆盖前会在本机留一份快照（localStorage `blf_inventory_v1_prev`）；只有点「强制下载」才用云端硬覆盖本机
- 两端同时改同一条数据以「最后保存」为准，日志里能查到是谁改的
- 备用方案：设置页可切到「GitHub Gist」，用私有 Gist 存数据（Token 只存本机浏览器，不写进代码）

## 五、必须知道的限制

- 云端连不上时，数据仍在**各自浏览器**里（localStorage），不会自动互通；联网后点一次「上传 / 下载」即可对齐。
- 每台设备仍建议定期「导出数据备份」。
- GitHub Pages 与 supabase.co 都是境外节点，国内访问速度一般，个别网络可能打不开（换手机流量试，或告诉我换国内方案）。
- 默认账号 `admin / admin123`、`zhangku / 123456`、`lidi / 123456`，上线后请到「权限管理」里改掉或新增自己的账号。

## 六、安全提醒

- 代码里的 `sb_publishable_...` 是 Supabase 的**公开** key，本来就给前端用；权限由数据库策略限定为只允许读写房间码 `blf-7k29xq4m`。`sb_secret_` 那行绝不能贴进代码或发人。
- 页面地址 + 房间码被人知道，理论上能读写这一份库存；要更严就换房间码，并在 Supabase 里同步改策略。
- Gist 方案下，Token 只保存在各自设备的浏览器里，别发给别人。
