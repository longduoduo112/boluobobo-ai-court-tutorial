# Hermes Personalities — 朝廷角色人设

本目录是 OpenClaw `agents.list[]` 在 Hermes Agent 中的等价物。

## 用法

```bash
mkdir -p ~/.hermes/personalities
cp configs/hermes/personalities/*.md ~/.hermes/personalities/

# 启动 Hermes 后
/personality silijian   # 切换到司礼监
/personality neige      # 切换到内阁
/personality duchayuan  # 切换到都察院
```

## 已提供的全部 19 角色

| 文件 | 朝廷职位 | 职责 |
|---|---|---|
| **核心调度** | | |
| `silijian.md` | 司礼监 | 任务调度中枢(默认人设,推荐做主入口) |
| `neige.md` | 内阁 | Prompt 优化 + 执行计划生成 |
| `duchayuan.md` | 都察院 | 代码审查 + 质量评估 |
| **六部** | | |
| `bingbu.md` | 兵部 | 编码开发 |
| `hubu.md` | 户部 | 财务分析 |
| `libu.md` | 礼部 | 品牌营销 |
| `gongbu.md` | 工部 | 运维部署 |
| `libu2.md` | 吏部 | 项目管理 |
| `xingbu.md` | 刑部 | 法务合规 |
| **翰林院 5 子角色** | | |
| `hanlin_zhang.md` | 翰林学士掌院 | 翰林院总管 |
| `hanlin_xiuzhuan.md` | 修撰 | 起草 |
| `hanlin_bianxiu.md` | 编修 | 修订 |
| `hanlin_jiantao.md` | 检讨 | 校对 |
| `hanlin_shujishi.md` | 庶吉士 | 见习研究 |
| **侍奉** | | |
| `qijuzhu.md` | 起居注官 | 朝廷流水/日报 |
| `guozijian.md` | 国子监 | 教育/培训 |
| `taiyiyuan.md` | 太医院 | 健康/告警 |
| `neiwufu.md` | 内务府 | 内务/采购 |
| `yushanfang.md` | 御膳房 | 膳食/食谱 |

## 想要单独迁移已有 OpenClaw 配置?

```bash
hermes claw migrate
```

会读 `~/.openclaw/openclaw.json` 里的 `agents.list[].identity.theme`,
转换成 personality 文件落到 `~/.hermes/personalities/`(若已有同名 .md 通常会跳过)。

## 想自己重新从 openclaw.example.json 生成?

```bash
python3 -c "
import json
with open('openclaw.example.json') as f:
    d = json.load(f)
for ag in d['agents']['list']:
    aid = ag['id']
    name = ag['name']
    theme = ag.get('identity',{}).get('theme','')
    fname = f'{aid}.md'
    with open(fname, 'w') as out:
        out.write(f'---\nname: {aid}\ndisplay_name: {name}\n---\n\n{theme}\n')
    print(f'wrote {fname}')
"
```

把生成的所有 `*.md` 拷到 `~/.hermes/personalities/` 即可。

## 字段约定

每个文件用 YAML frontmatter:
- `name`: personality id(切换时用,英文 snake_case)
- `display_name`: 显示名(中文)

frontmatter 之后是 system prompt 正文。
