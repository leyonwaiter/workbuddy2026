# workbuddy2026 · 成果交付仓库说明

> 本仓库是「成果交付中心」。每个**成型可用**的项目，都把它打包推成 **Release** 挂到这里；主分支只放展示壳，不存源码。任何 AI 启动新项目时，先来读这份说明，照着把成果推上来即可。

---

## 一、仓库结构

| 位置 | 内容 | 说明 |
|---|---|---|
| 主分支 `main` | `index.html`（展示页）+ 本 `README.md`（本说明） | 壳 + 入库 SOP |
| **Releases** | 各项目的打包 zip / 文档 | **真·成果都在这里** |

`index.html` 打开后通过 GitHub API 自动拉取本仓库全部 Release，按 tag 里的项目名分组展示，每条带下载按钮。主分支没有源码树，克隆也只拿到壳。

---

## 二、分组规则（tag 命名）

Release 的 tag 格式固定为：`<项目名>-v<YYYY.MM.DD.xxxx>`

- 展示页从**最后一个 `-v`** 切开归类：`jieqi-v2026.10.08.2206` → 归入项目 **jieqi**
- 同一项目多次发布，换日期/序号即可，自动归到同一组
- 项目名只用英文/拼音，别带空格和中文，避免分组错乱

---

## 三、推送 SOP（AI 照做，已实测跑通）

**前置：登录 GitHub**（没 PAT 就问用户要一个，勾 `repo` 权限）
```bash
gh auth login --with-token      # 把用户给的 PAT 从 stdin 喂进去
gh auth status                  # 确认已登录 leyonwaiter
```

**1. 打包项目（务必剔除密钥）**
```bash
zip -r 项目名-release.zip 项目目录 -x '.git/*' 'account.json' '*.secret' '轮询*.txt' 'poll.log'
```
- 含密钥的文件（AppID/AppSecret/token，如 `account.json`）**严禁入库**
- 本地用 `.gitignore` 挡死，仓库里只留 `*.example.json` 占位模板
- 打包后核验：`unzip -l 项目名-release.zip | grep -c account.json` 应为 `0`

**2. 发 Release（不碰主分支，只发附件）**
```bash
gh release create 项目名-v2026.10.08.xxxx 项目名-release.zip \
  --repo leyonwaiter/workbuddy2026 \
  --title "项目名 项目名-v2026.10.08.xxxx" \
  --notes "（一句话说明这包含什么、怎么用）"
```

**3. 校验**
```bash
gh release view 项目名-v2026.10.08.xxxx --repo leyonwaiter/workbuddy2026 --json tagName,assets
```
返回 `assets[0].state: uploaded` 即成功，页面上立即出现。

---

## 四、密钥处理铁律

- 任何含密钥的文件（微信 AppID/AppSecret、各类 token、API key）**不得进 GitHub**
- 本地 `.gitignore` 排除 + 仓库只留 `*.example.json` 占位
- **若项目需要把密钥交付给用户**：不要推 GitHub，改为发到用户**邮箱**（见第五节问法），GitHub 上只放脱敏后的产物

---

## 五、AI 启动新项目时的动作序列

1. **先访问本仓库**（读这份 README + 看 `index.html` 展示页），确认交付规范
2. 项目成型后，按「三、推送 SOP」打包发 Release，tag 按「二」命名
3. 过程中**只可能问用户两件事**，其余自行决断、别每步打扰：
   - ① 涉及**密钥交付**（要发邮箱）→ 问用户**邮箱地址 / 密钥怎么发**
   - ② 没登录 GitHub → 问用户要 **GitHub PAT（API key）**
4. 推上去、页面显示出来，交付即算完整

---

## 六、已发布项目

| 项目 | tag | 内容 |
|---|---|---|
| jieqi | `jieqi-v2026.10.08.2206` | 廿四节气微信推送流水线（scripts/docs/4篇稿，密钥已剔） |
| publish | `publish-v2026.10.08.2220` | 发布为应用使用说明 |
| japanese-imageguide | `japanese-imageguide-v2026.10.08.224915` | 日语学习文章配图方案交接包（SKILL 复刻用，零密钥） |
| downloadpage | `downloadpage-v2026.10.12.1708` | 浏览器原生下载改进版 · 成果展示/公网交付工具 |

---

> 铁律一句话：给我看东西走「发布为应用」出公网链接；成果入库走 `gh release create` 发到本仓库、只发 Release 不碰主分支、密钥绝不进 GitHub。
