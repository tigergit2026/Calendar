# 稿多多校历查询

澳洲 24 所 + 英国 20 所高校的**当前教学周实时查询**。纯静态单页站点，零依赖、零构建、可直接托管在 GitHub Pages。

- **第 1 层 · 入口页**：选择地区（澳洲 / 英国）
- **第 2 层 · 校历页**：北京时间大时钟 → 搜索学校（简称 / 中文名 / 城市 / 国家 / 别名）→ 各校卡片（状态徽章、`Week N / 总周数`、与北京时差、退课截止日、成绩公布日、当期学期时间总览表）→ 官方校历链接

---

## 在线访问

部署完成后地址为：

```
https://<你的 GitHub 用户名>.github.io/gaoduoduo-calendar/
```

- 澳洲：`…/gaoduoduo-calendar/au.html`
- 英国：`…/gaoduoduo-calendar/uk.html`

> 国内访问 `*.github.io` 可能不稳定，如需提速可参考文末「可选：套 Cloudflare Pages / 自定义域名」。

---

## 文件结构

| 文件 | 说明 | 大小 |
|---|---|---|
| `index.html` | **入口页（登录页）**，无日历引擎，秒开 | ~14 KB |
| `au.html` | 澳洲高校教学周日历查询（24 所） | ~77 KB |
| `uk.html` | 英国高校教学周日历查询（20 所） | ~63 KB |
| `404.html` | 未知路径兜底，3 秒自动跳回入口 | ~13 KB |
| `.nojekyll` | 关闭 GitHub Pages 的 Jekyll 处理 | 0 |

三个页面自包含（CSS / JS / 数据全部内联），**没有任何外部请求**，可离线双击打开（此时入口页的跳转同样有效）。

---

## 本地预览

```bash
# 直接双击 index.html 即可
# 或起一个本地服务（推荐，行为与线上完全一致）
cd gaoduoduo
python3 -m http.server 8080
# 打开 http://localhost:8080
```

---

## 部署到 GitHub Pages

### 第 1 步 · 在 GitHub 上新建仓库

打开 https://github.com/new ，填写：

- **Repository name**：`gaoduoduo-calendar`（可自定义）
- **Visibility**：`Public`（免费版 GitHub Pages 需要公开仓库）
- **不要**勾选 Add a README / .gitignore / license（保持空仓库，避免首次推送冲突）

### 第 2 步 · 推送代码

在 `gaoduoduo` 目录下执行（把 `<你的用户名>` 换成实际用户名）：

```bash
cd gaoduoduo

git init -b main
git config user.name  "你的名字"          # 仅本仓库生效
git config user.email "你的GitHub邮箱"     # 建议用 GitHub 账号邮箱，提交才会关联到你的头像
git add -A
git commit -m "稿多多校历查询：澳洲 24 所 / 英国 20 所高校教学周查询"
git remote add origin https://github.com/<你的用户名>/gaoduoduo-calendar.git
git push -u origin main
```

> 本目录已经 `git init` 并完成了首次提交。如果你的 Git 身份需要更正，执行
> `git config user.name "…" && git config user.email "…" && git commit --amend --reset-author --no-edit`
> 即可重写这次提交的作者信息。

首次推送会要求认证。**GitHub 已不支持账号密码**，请用以下任一方式：

- **Personal Access Token**（最简单）：GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new token，勾选 `repo` 权限，复制后**当作密码粘贴**。
- **SSH**：把 `origin` 改成 `git@github.com:<你的用户名>/gaoduoduo-calendar.git`，并先配好 SSH key。
- **GitHub Desktop**：https://desktop.github.com ，图形界面直接 Add Local Repository → Publish。

### 第 3 步 · 开启 Pages

仓库页面 → **Settings** → 左侧 **Pages**：

- **Source**：`Deploy from a branch`
- **Branch**：`main` + `/ (root)`
- 点 **Save**

等待约 1 分钟，刷新该页面即会出现绿色提示与访问地址：

```
https://<你的用户名>.github.io/gaoduoduo-calendar/
```

### 第 4 步 · 以后更新

改完文件后：

```bash
git add -A
git commit -m "更新校历数据"
git push
```

推送后 1 分钟内自动生效（浏览器如看到旧内容，按 `Cmd + Shift + R` 强制刷新）。

---

## 可选：自定义域名 / 加速

**自定义域名**：在 Pages 设置页的 Custom domain 填入自己的域名（如 `cal.yourdomain.com`），然后在域名 DNS 处添加一条 CNAME 记录指向 `<你的用户名>.github.io`，并在仓库根目录放一个 `CNAME` 文件（内容就是该域名）。

**国内提速**：把本目录整体上传到 Cloudflare Pages（https://pages.cloudflare.com ，免费），Build command 留空、Output directory 填 `/`，即可获得较稳定的访问速度。

---

## 数据与维护

### 数据来源
全部日期取自各校**官网官方校历**，页脚列出每所学校的官方校历链接。带「参考」标记的学期表示官网尚未公布完整日期，起止按官方已公布的开学 / 结课日推算。

### 计算规则
- 教学周 = 官方教学起始日 → 教学结束日，逐周推进
- **休息周不计入教学周、不占教学周编号**（阅读周 / 巩固周 / 圣诞假期 / 复活节假期等），在汇总表中单独成组
- 时差徽章按当天实际时差动态显示：英国夏令时（BST）比北京时间晚 7 小时，冬令时（GMT）晚 8 小时
- 英国 19 所学校的退课截止日 / 成绩公布日官网未统一公布，标注「待官网公布」；Glasgow 等有官方硬日期

### 新增 / 修改学校
本目录的三个页面由**同一份母版**生成，母版与构建脚本位于本地工作区 `_src/`：

```
_src/master-single.html   ← 母版（CSS + 引擎 + 澳洲/英国全部数据 + 入口页）
_src/build_site.py        ← 构建脚本，产出 index.html / au.html / uk.html / 404.html
```

维护流程：

1. 编辑 `_src/master-single.html`：
   - 加学校 → 在 `const AU_SCHOOLS = [...]` 或 `const UK_SCHOOLS = [...]` 里照格式追加一条
   - 加学校链接 → 同步改 `const SITES = {...}` 里对应站点的 `links` 数组
2. 执行 `python3 _src/build_site.py` 重新生成三个页面
3. `git add -A && git commit && git push`

> 构建脚本自带完整性校验（占位符、链接目标、残留路由检查），不通过会直接报错退出。

---

## 浏览器支持

Chrome / Edge / Safari / Firefox 现代版本，以及 iOS Safari、Android Chrome。响应式：≥1080px 三栏、≤1080px 两栏、≤640px 单栏。

---

## 免责声明

本站为**非官方**查询工具，数据整理自各校公开校历。教学周、假期、截止日期可能因学院、专业、课程层级（本科 / 研究生）而异，**请以学校官网与 AU / UK 官方通知为准**。因使用本站信息产生的任何后果，本站不承担责任。
