# 稿多多校历查询

澳洲 24 所 + 英国 20 所 + 新西兰 8 所 + 新加坡 3 所院校的**当前教学周实时查询**。纯静态单页站点，零依赖、零构建、可直接托管在 GitHub Pages。

- **第 1 层 · 入口页**：选择地区（澳洲 / 英国 / 新西兰 / 新加坡），宽屏 2×2 卡片
- **第 2 层 · 校历页**：北京时间大时钟 → 搜索学校（简称 / 中文名 / 城市 / 国家 / 别名）→ 各校卡片（状态徽章、`Week N / 总周数`、与北京时差、退课截止日、成绩公布日、当期学期时间总览表）→ 官方校历链接

---

## 在线访问

部署完成后地址为：

```
https://<你的 GitHub 用户名>.github.io/gaoduoduo-calendar/
```

- 澳洲：`…/gaoduoduo-calendar/au.html`
- 英国：`…/gaoduoduo-calendar/uk.html`
- 新西兰：`…/gaoduoduo-calendar/nz.html`
- 新加坡：`…/gaoduoduo-calendar/sg.html`

> 国内访问 `*.github.io` 可能不稳定，如需提速可参考文末「可选：套 Cloudflare Pages / 自定义域名」。

---

## 文件结构

| 文件 | 说明 | 大小 |
|---|---|---|
| `index.html` | **入口页（登录页）**，无日历引擎，秒开 | ~15 KB |
| `au.html` | 澳洲大学教学周日历查询（24 所） | ~81 KB |
| `uk.html` | 英国大学教学周日历查询（20 所） | ~66 KB |
| `nz.html` | 新西兰大学教学周日历查询（8 所） | ~65 KB |
| `sg.html` | 新加坡大学教学周日历查询（3 所院校） | ~57 KB |
| `404.html` | 未知路径兜底，3 秒自动跳回入口 | ~13 KB |
| `.nojekyll` | 关闭 GitHub Pages 的 Jekyll 处理 | 0 |

五个页面自包含（CSS / JS / 数据全部内联），**没有任何外部请求**，可离线双击打开（此时入口页的跳转同样有效）。

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
git commit -m "稿多多校历查询：澳洲 24 所 / 英国 20 所 / 新西兰 8 所 / 新加坡 3 所院校教学周查询"
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
- **休息周不计入教学周、不占教学周编号**（阅读周 / 巩固周 / 期中假 / 圣诞假期 / 复活节假期等），在汇总表中单独成组
- 时差徽章按当天实际时差动态显示：英国夏令时（BST）比北京时间晚 7 小时、冬令时（GMT）晚 8 小时；新西兰夏令时（NZDT）比北京时间早 5 小时、冬令时（NZST）早 4 小时
- 英国 19 所学校的退课截止日 / 成绩公布日官网未统一公布，标注「待官网公布」；Glasgow 等有官方硬日期
- 新西兰各校无 census date 制度，退课截止 / 出分日多数标注「待官网公布」；坎特伯雷（S1 5.10 / S2 9.27）、奥塔哥（S2 9.20）、VUW（T2 出分 11.20）、梅西（出分 7.3 / 12.1）、林肯（S1 3.6 / 出分 7.8、11.20）为官网硬日期
- 新加坡全境 Asia/Singapore（UTC+8）**与北京时间完全相同**、无夏令时，时差徽章统一显示「与北京时间同时」；SIM 采用「学习期（Study Period）」制且不公布 census / 出分以外的节点，退课截止日多标注「待官网公布」；科廷新加坡 2A 的 census（退课截止）为官网硬日期 7.24

### 新西兰学期制说明
新西兰是南半球学年制（2 月开学、11 月结束）：
- **两学期制（Semester 1 / 2）**：奥克兰、奥塔哥、梅西、坎特伯雷、林肯、AUT
- **三段学期制（Trimester A / B / C）**：怀卡托；**Trimester 1 / 2 / 3**：惠灵顿维多利亚
- 夏季学期（Summer School）跨年，多数学校 11 月开课、次年 2 月结束，汇总表中按独立学期列出
- 期中假（Mid-semester break / Mid-term break / Mid-trimester break）与 Study break 均按休息周处理，不计入教学周

### 新加坡学制说明
新加坡三所院校学制差异较大，本站按各自官方结构分别建模：
- **新加坡管理学院（SIM）**：**6 个学习期（Study Period 1–6）**制，每期约 6 教学周，期与期之间为假期；每期标注 Orientation Week Start、Commencement Date、Results Release。SIM 与 RMIT、伦敦大学、伍伦贡大学、莫纳什学院等合作办学的课程另有各自的学期安排，以学院发放的 programme academic calendar 为准
- **科廷新加坡（Curtin Singapore）**：**12 教学周 + 1 学习周 + 2 考试周**的三学期制（Trimester 1A / 2A / 3A），学年末跨年（3A 至次年 2 月）。另设有 Curtin College 文凭课程与 Navitas 语言课程，学期起止不同
- **詹姆斯库克大学新加坡校区（JCU Singapore）**：**10 教学周 + 期中 Study Week + 复习 Study Week** 的三学期制（Trimester 1S / 2 / 3），期中学习周与农历新年假期作为休息周处理，不计入教学周

### 新增 / 修改学校
本目录的四个页面由**同一份母版**生成，母版与构建脚本位于本地工作区 `_src/`：

```
_src/master-single.html   ← 母版（CSS + 引擎 + 澳洲/英国/新西兰/新加坡全部数据 + 入口页）
_src/build_site.py        ← 构建脚本，产出 index.html / au.html / uk.html / nz.html / sg.html / 404.html
```

维护流程：

1. 编辑 `_src/master-single.html`：
   - 加学校 → 在 `const AU_SCHOOLS = [...]`、`const UK_SCHOOLS = [...]`、`const NZ_SCHOOLS = [...]` 或 `const SG_SCHOOLS = [...]` 里照格式追加一条
   - 加学校链接 → 同步改 `const SITES = {...}` 里对应站点的 `links` 数组
2. 执行 `python3 _src/build_site.py` 重新生成全部页面
3. `git add -A && git commit && git push`

> 构建脚本自带完整性校验（占位符、链接目标、残留路由检查），不通过会直接报错退出。

---

## 浏览器支持

Chrome / Edge / Safari / Firefox 现代版本，以及 iOS Safari、Android Chrome。响应式：入口页四张卡片 ≥861px 为 2×2 两列、≤860px 单列；校历页 ≥1080px 三栏、≤1080px 两栏、≤640px 单栏。

---

## 免责声明

本站为**非官方**查询工具，数据整理自各校公开校历。教学周、假期、截止日期可能因学院、专业、课程层级（本科 / 研究生）而异，**请以学校官网与 AU / UK / NZ / SG 官方通知为准**。因使用本站信息产生的任何后果，本站不承担责任。
