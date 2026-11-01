# ChildrenSong：儿歌 MV 制作范本

这个仓库是儿歌 MV 的**制作范本**。每支新作品的歌、时长、场景和动作都不一样，但**风格、角色、剪辑规则和质量**必须和样板成片一致。

> **核心一句话：先有歌，再有画面。一句歌词一个镜头，唱什么就演什么，切点卡在歌词上。**

工具链：Suno 做歌 → 图片模型做关键帧 → Hailuo 图生视频 → Python + ffmpeg 剪辑。

---

## 0. 开工前先看（按顺序）

1. **`作品集/` 里的成片**：质量和风格的标准。当前样板是 `作品集/PandaBobo_2026-11-01.mp4`；有多支时以日期最新的为准。
2. **`人物/`**：角色设计图，每个角色一张 turnaround 和一张 expressions。
3. **`人物/characters.json`**：每个角色的外观描述和参考图路径，写提示词时原文照抄。仓库里没有镜头表，每支作品的 `shots.json` 由 agent 自己建，见第 10 节第 2 步。
4. **[PRODUCTION_GUIDE.md](PRODUCTION_GUIDE.md)**：完整准则，包括歌曲和合规。
5. **[animation_plan.md](animation_plan.md)**：动作手法，包括挤压拉伸、预备过冲、节拍脉冲。只学手法，不照搬里面的段落表。

## 1. 工作范畴：什么固定，什么每支作品自己定

| 固定不变（所有作品一致，要改先问负责人） | 每支新作品自己定 |
|---|---|
| 角色外观（`人物/` 里的参考图和 `characters.json`） | 歌曲、歌词、BPM、片长 |
| 画面风格和提示词（第 5 节） | 段落结构和镜头数量 |
| 剪辑规则：一句一镜、切点卡歌词、段间遮挡转场（第 2 节） | 每句歌词的动作，按歌词内容重新设计 |
| 成片规格：1920×1080、24fps | 场景，可以沿用旧场景，也可以新建 |
| 制作顺序、上传规则和自检清单（第 3、11、12 节） | 用哪个遮挡转场道具 |

**作品集里的成片只是质量和风格的标准，不是要照抄的内容。** 不要照搬它的时长、镜头数、动作或场景顺序；这些都要从新歌的歌词和节拍推出来。

## 2. 已定决策（要修改先问负责人）

1. **剪辑结构**：段内一句一镜，用硬切；段间用遮挡转场。不做整首一镜到底。
   - 切点卡歌词：切在这个镜头所演那句歌词的第一个词上，提前 1 帧。句中的反应镜头切在拍上。
   - 遮挡转场：花丛、树叶、水花、云朵或气球盖满镜头，揭开后就是新场景。每个段落交界最多 1 处，整首最多 5 处。
2. **片段长度**：按 2 或 4 小节生成。1 小节 = 4 × 60 ÷ BPM 秒（140 BPM 时 1 小节 1.71 秒，2 小节 3.43 秒）。模型输出比需要的长时，裁到整小节。
3. **生成方式**：一律图生视频，只给首帧（这个镜头的关键帧），原速播放，不变速、不补帧，不用首尾帧链。
4. **成片规格**：1920×1080，24fps。模型只能出 768p 时，在剪辑的最后一步放大（lanczos 加轻度锐化）。暂不做 4K。

## 3. 制作顺序（不可颠倒）

1. **歌**：Suno 生成，实测 BPM，按段落切分（前奏 / 主歌 / 副歌 / 尾声）。片长跟着歌走，不固定。
2. **分镜表**：按第 6 节的模板，一行一句歌词。没有分镜表不准开始生成。
3. **关键帧**：每个镜头一张首帧，角色和场景都要对照参考图。
4. **片段**：每张首帧生成 2–3 个候选，挑最好的一个。
5. **剪辑**：按第 10 节第 7 步拼接，加音效和卡拉 OK 字幕。
6. **自检和上传**：逐条过第 12 节清单，通过后按第 11 节上传。

## 4. 角色

| 角色 | 主色 | 识别点 |
|---|---|---|
| 狮子 lion | 橙色鬃毛、黄色身体 | 蓬松的圆形鬃毛，头顶一撮黄色卷毛；蓝绿色背带短裤，胸袋绣着黄色太阳 |
| 水獭 otter | 棕色 | 奶油色脸和肚子，有胡须；珊瑚橙背心配黄扣子，尾巴又长又粗 |
| 大象 elephant | 淡紫色 | 大耳朵内侧是粉色，头顶一撮紫毛；淡黄色连体衣，奶油色波浪领 |

规则：

1. 外观只以 `人物/` 参考图和 `人物/characters.json` 为准，不要临时改描述，也不要自己加配饰。
2. **歌词里的主角必须是画面里的主角**。歌词唱"我是熊猫"，画面里就必须是熊猫。
3. 要加新角色：先做 turnaround 和 expressions 两张参考图，在 `人物/characters.json` 写好外观描述，负责人确认后才能用。
4. 造型：大头大眼，圆润，没有尖角，像软胶毛绒玩具；一个角色一个主色。

## 5. 画面风格

### 5.1 提示词（原文照抄，不要改写）

**关键帧**：

```
a frame from a high-end 3D animated preschool music video, 2.5D plush toy characters with soft felt fur exactly matching the reference character sheets (same proportions, outfits, colours and faces), background matches the reference scene image (same place, art style, colours and lighting), soft warm sunlight, clean readable composition, sharp focus on the characters, caught mid-motion, no text, no watermark, no extra characters, 16:9
```

**视频**：

```
One continuous shot with no cuts. Smooth natural motion at normal real-time speed, bouncy and rhythmic like dancing to a cheerful song at about two bounces per second. The characters keep exactly the same faces, proportions, colours and outfits as in the first frame. The background stays solid and unchanged. High quality 3D animated plush toy style, soft lighting.
```

唯一可以改的地方是 `about two bounces per second`：歌速在 120–150 BPM 之间不用改；超出这个范围就改成大约 BPM ÷ 60 次，例如 90 BPM 写 `about one and a half bounces per second`。

**场景**：

```
storybook 3D environment, children's animation background, same art style, colours and lighting as the reference scene, soft warm sunlight, saturated but soft pastel colours, rounded clean shapes, open empty ground for characters, no characters, no animals, no people, no text, no watermark
```

**遮挡转场道具**：

```
3D render in the same soft plush storybook animation style, soft warm lighting, transparent background, no ground, no shadow, no text
```

视频提示词里还必须带上：`static camera, characters look at camera, bouncy cartoon motion, same look and outfits, no morphing`

### 5.2 镜头

1. 只用三种景别：正面中景、面部特写、全景。机位放在儿童视线高度。
2. 默认固定机位，最多加慢推、慢拉或慢摇。不要环绕、甩镜或乱晃。
3. 角色常看向镜头，像在和小孩说话。
4. 一个片段只做一件事：一个动作、一个镜头，片段里不换场景。

### 5.3 场景与色彩

1. 每支作品用 4–6 个场景，在作品内反复使用。场景按歌词内容选，可以沿用旧场景，也可以新建。
2. 新场景先用场景提示词生成一张空场景图，作为这个场景所有关键帧的参考。
3. 高饱和暖色，光线明亮均匀，背景干净，给角色留出空地。不要偏灰。
4. 所有镜头套同一个调色：暖色、高饱和、轻微暗角。
5. 粒子配色：粉 #FF6B8B、黄 #FFD23F、蓝 #4DC9FF、紫 #9B7BFF、橙 #FF9F43、绿 #6BE585。

## 6. 分镜表（最关键）

唱到什么，画面就演什么。每支新作品都先写分镜表，一行一句歌词。格式如下，`< >` 里的内容按新歌填写：

| # | 歌词 | 时长(拍) | 动作 | 镜头 | 音效 |
|---|---|---|---|---|---|
| 1 | <第 1 句歌词> | <拍数> | <谁做什么，按这句歌词设计> | <景别，机位运动> | <音效 × 次数> |
| 2 | <第 2 句歌词> | <拍数> | <…> | <…> | <…> |

规则：

1. **动作从歌词推出来**：歌词说拍手就拍手，说转圈就转圈。不要从作品集或旧作品照搬动作。
2. 每句歌词至少对应一个**可以模仿的动作**，越简单越好，2 岁小孩也能做。
3. 手势要具象：大小、上下、左右都用身体表现出来。
4. 每集一个小故事：先出一个小问题，再用一首歌解决，最后以拥抱或笑脸收尾。
5. 段落交界那一行，在"镜头"栏写明遮挡转场用哪个道具。

## 7. 剪辑节奏（按比例，不按秒数）

1. **镜头数 ≈ 歌词句数**，可以加少量反应镜头。片长和镜头数都由新歌决定。
2. 每个镜头 1–2 小节，一般不超过 4 小节。参考范围：每分钟切换 15–30 次。
3. 开头不放片头，第 1 秒就进歌或进画面。
4. 90% 以上用硬切，特效转场只用在段落交界。
5. 每个镜头至少一个音效（拍手、水声、笑声、"叮"、落地声），放在动作发生的那一帧上。
6. 歌词字幕用卡拉 OK 样式逐词变色，字体圆润、字号要大。

## 8. 生成与筛片

1. 一律图生视频，用角色参考图做出的首帧，不用纯文生视频。
2. 动作太慢、太飘：在提示词里加 `normal real-time speed, bouncy and rhythmic` 后重新生成。不要靠变速或补帧去救。
3. 角色变形、脸变了、背景扭曲：直接重新生成，不要用剪辑去救。

## 9. 制作环境搭建

仓库里不放代码。每个 agent 按本节在本地搭好环境，再按第 10 节自己写脚本。

### 9.1 软件

1. **Python 3.11 以上**，安装：`pip install numpy opencv-python pillow librosa faster-whisper requests`
2. **ffmpeg**：要带 libx264 和 libass（烧字幕用）。用 `ffmpeg -version` 确认。
3. **Suno**：歌由人在 Suno 网站上做好，导出 WAV 或 MP3 交给 agent。agent 不负责做歌。

### 9.2 模型和 API

图片和视频都通过 OpenRouter 调用，API key 放在环境变量 `OPENROUTER_API_KEY` 里。**key 不能写进任何文件，也不能上传。**

| 用途 | 模型 | 接口 | 参数 |
|---|---|---|---|
| 场景图、关键帧、转场道具 | `openai/gpt-image-2.5-sunburst` | `POST https://openrouter.ai/api/v1/images` | 16:9，quality medium；道具加 `background: transparent` |
| 视频片段 | `minimax/hailuo-3-max` | `https://openrouter.ai/api/v1`（提交任务 → 轮询 → 下载） | 5 秒，768p，16:9，只给首帧 |

1. 视频约 0.08 美元 / 秒，一个 5 秒片段约 0.4 美元。开工前先设预算上限，每次调用前检查，超出就停。
2. 每次付费调用都记一行账（时间、镜头编号、费用），写在本地。

### 9.3 本地工作目录

每支作品一个文件夹，**整个文件夹不上传**（加进 `.gitignore`）：

```
works/<作品名>/
├─ song.wav            歌
├─ lyrics.txt          歌词，按段落分
├─ storyboard.md       分镜表（第 6 节格式）
├─ shots.json          镜头表（第 10 节第 2 步，agent 自己建）
├─ timeline.json       节拍和每句歌词的开始时间
├─ scenes/             空场景图
├─ keys/               关键帧 K01.png …
├─ clips/              片段 K01.mp4 …（包括候选）
├─ props/              遮挡转场道具（透明 PNG）
└─ out/                成片和联系表
```

## 10. 制作流程：每一步怎么做

1. **节拍和歌词时间**：用 librosa `beat_track` 求出每一拍和每小节第一拍；用 faster-whisper（`small.en`，打开 `word_timestamps`）求出每个词的时间，再对齐到 `lyrics.txt`，得到每句歌词第一个词的时间。结果写进 `timeline.json`。重复的副歌和数数这类 whisper 容易漏的地方，用节拍补齐，并人工听一遍确认。
2. **分镜表和镜头表**：先按第 6 节写 `storyboard.md` 给人看，再由 agent 自己转成 `shots.json` 给脚本用。镜头编号用 K01、K02…，一句歌词一个镜头。格式如下，`< >` 里的内容按新歌填写：

   ```json
   {
    "work": "<作品名>",
    "bpm": "<实测 BPM>",
    "scenes": {
     "S1": {"desc": "<场景描述，用来生成空场景图>", "image": "scenes/S1.png"}
    },
    "shots": [
     {
      "id": "K01",
      "t": "<计划开始时间，秒>",
      "line": "<这一镜演的那句歌词>",
      "section": "<Intro / Verse 1 / Chorus / Outro …>",
      "scene": "S1",
      "chars": ["<characters.json 里的角色名>"],
      "expr": "<表情，对照 expressions 图>",
      "frame": "<景别和机位>",
      "action": "<首帧里角色正在做什么>",
      "motion": "<片段里的动作，按这句歌词设计>",
      "cam": "static camera",
      "in": null
     }
    ]
   }
   ```

   - `chars` 只能填 `人物/characters.json` 里有的角色名。
   - `cam` 只用：`static camera`、`slow push in`、`slow pull back`、`slow tilt up`。
   - `in` 是进入这一镜的遮挡转场，只在段落交界填（`leaves`、`bush`、`splash`、`cloud`、`balloons`），其余一律 `null`，也就是硬切。
3. **场景图**：每个场景用场景提示词生成一张空场景图，放进 `scenes/`。
4. **关键帧**：提示词 = 景别和机位 + 这一镜的动作 + 关键帧提示词；参考图 = 场景图 + 出场角色的 turnaround 和 expressions。同一场景的后续镜头，可以把这个场景第一镜的关键帧也作为参考，让画面更一致。
5. **片段**：提示词 = 动作描述 + `Camera: <机位运动>.` + 视频提示词；只给首帧。每镜生成 2–3 个，按第 8 节筛选。
6. **遮挡道具**：用道具提示词生成透明背景 PNG（树叶、花丛、水花、云、气球），放进 `props/`。
7. **剪辑**：自己写一个 Python 脚本，用 ffmpeg 读帧、写帧，规则如下：
   - **切点**：分镜表上这一镜的计划时间，如果一拍之内有一句歌词开始，就切在这句的第一个词上；否则切在最近的一拍上。所有切点提前 1 帧。
   - **取哪一段**：从片段里截一段原速画面，长度等于这个镜头的时长。挑的规则：尽量靠前（离首帧越近越不走样）；避开画面突然跳变的帧（相邻帧差值突然暴增，往往是模型偷偷切了镜头）；动作的发力点尽量落在拍上；切走时角色还在动。
   - **调色**：每个片段算一次固定的 RGB 增益，让第一帧的颜色贴近关键帧，整段用同一组数值，不逐帧变化。再统一套暖色、高饱和、轻微暗角。
   - **放大**：`scale=1920:1080:flags=lanczos,unsharp=5:5:0.4`，输出 24fps。
   - **转场**：只在段落交界做遮挡转场。道具在切点前后各 10 帧内扫过镜头，切点那一帧必须完全盖住画面；段内一律硬切。
   - **字幕和收尾**：卡拉 OK 字幕做成 ASS 文件，用 ffmpeg 的 `subtitles` 滤镜烧进去；开头淡入 0.3 秒，结尾淡出 0.9 秒。
   - **编码**：libx264，CRF 17，yuv420p，加 `-movflags +faststart`；音频用 AAC 192k。
   - **检查图**：在每个切点前后各截一帧，拼成联系表，用来检查变形和跳帧。

## 11. 上传规则：每次只上传最终作品

1. **只上传一样东西：成片**，放进 `作品集/`。关键帧、片段、道具、脚本、分镜表和日志都只留在本地 `works/` 里，不上传。
2. **命名**：`作品名_YYYY-MM-DD.mp4`，日期是制作完成并上传的那一天，例如 `PandaBobo_2026-11-01.mp4`。作品名用英文，不加空格，不加 `final`、`v5` 这类版本词。
3. **大小**：GitHub 单个文件上限 100MB。超过时用 ffmpeg 两遍编码压到 95MB 以下，保持 1920×1080、24fps。视频码率 ≈ 95 × 8 ÷ 片长秒数 Mbps，再减去音频的 0.2 Mbps。
4. **锁定文件**：`人物/`（含 `characters.json`）、本文件、PRODUCTION_GUIDE.md 第 0 节。要改先问负责人。
5. **一支作品一个 PR**：PR 里只加 `作品集/` 下的那一个 mp4，说明里贴上第 12 节自检清单的结果。

### 11.1 分支规则：不影响、不覆盖别人的作品

1. **不准直接改 `main`**：不往 `main` 提交，也不往 `main` 推送。所有作品都走"开分支 → PR → 负责人合并"。
2. **开工前先同步，再开分支**：

   ```bash
   git checkout main
   git pull origin main
   git checkout -b work/<作品名>_<YYYY-MM-DD>
   ```

   一支作品一个分支，分支名和成片同名，例如 `work/PandaBobo_2026-11-01`。不要在别人的分支上提交。
3. **只新增，不改动**：分支里只能**新增**自己的那一个 mp4。不准修改、删除、改名或替换 `作品集/` 里已有的文件，也不准动 `人物/` 和规则文件。如果 `作品集/` 里已经有同名文件，就停下来问负责人，不要覆盖。
4. **提交前检查**：执行下面的命令，结果必须**只有一行**，并且以 `A` 开头（A 表示新增）：

   ```bash
   git diff --name-status origin/main...HEAD
   ```

   正确的结果长这样：`A	作品集/<作品名>_<YYYY-MM-DD>.mp4`。多出任何一行，或者出现 `M`（修改）、`D`（删除）、`R`（改名），就先撤销那些改动再提交。
5. **推送和合并**：
   - 用 `git push -u origin work/<作品名>_<YYYY-MM-DD>` 推送分支，再开 PR 到 `main`。
   - 合并只由负责人做，agent 不自己合并。
   - 不准用 `git push --force`。
   - `main` 在这期间有更新时，执行 `git pull origin main` 合到自己的分支上，再重做第 4 步的检查。

## 12. 交付前自检

任何一条不通过都不发布。没有验证的条目要明确说出来：

- [ ] 角色在所有镜头里外观一致，没有变形；歌词里的主角就是画面里的主角
- [ ] 每句歌词都有对应动作，动作是按这首歌的歌词设计的；切点卡歌词；特效转场只出现在段落交界
- [ ] 每个镜头都有音效；字幕是卡拉 OK 样式
- [ ] 成片 1920×1080、24fps、不超过 100MB，音画同步，第 1 秒就进内容；文件名符合第 11 节
- [ ] 没有危险动作；版权干净；AI 生成按平台要求披露；Made for Kids 已勾选（详见 PRODUCTION_GUIDE.md 第 8 节）
