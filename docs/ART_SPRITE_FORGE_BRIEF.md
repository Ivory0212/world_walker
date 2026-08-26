# 霧界繪行者：Agent Sprite Forge 美術製作規格

此文件給 Codex / agent-sprite-forge 使用。目標不是產生零散漂亮圖片，而是建立可直接進遊戲、風格一致、可商用整理的 2D 像素資產管線。

## 核心視覺
- 俯視 3/4 視角，接近 16/32-bit JRPG 高細節像素美術。
- 可愛但不幼兒化；世界有廢墟、自然侵蝕、神秘遺跡與遠古科技。
- 場景密度高：樹冠、岩壁、草叢、岸線、遺跡、殘骸、水面細節都要豐富。
- 不做純平面棋盤感，不做手遊節點地圖。
- 色彩分區明確：地表明亮、洞窟偏冷暗、地堡偏鐵鏽與青灰、海窟偏青藍、天空遺跡高明度冷色。

## 輸出規格
- Tile 基準：32x32 px；所有地形需可無縫拼接。
- 角色：32x48 或 48x64，四方向 idle/walk；每方向至少 4 frame。
- 戰鬥近景角色：96x96 以上，idle / attack / hurt / cast。
- Monster：32x32～96x96；Boss 可到 192x192。
- 透明 PNG；nearest-neighbor；禁止 JPEG artifact。
- 命名規則：assets/{category}/{biome}_{object}_{variant}_{state}.png

## 第一批必做 Tileset
1. mistport_coast_tileset：海、水、沙、草、泥地、岸線轉角、淺水、礁石。
2. mistport_forest_tileset：闊葉樹、灌木、花、倒木、藤蔓、林間陰影。
3. ridge_tileset：山壁、山洞入口、懸崖、裂谷、碎石、斜坡。
4. echo_cave_tileset：岩地、晶簇、石柱、地下水、崩塌隧道、測繪匣。
5. bunker_tileset：軍用牆體、防爆門、管線、鏽蝕地板、控制台、照明。
6. tide_cave_tileset：潮池、濕岩、海草、沉船殘骸、潮汐機關。

## 第一批角色
- 燕：斥候，深藍短披風，黃銅測繪工具，輕裝。
- 祈燈：繪界師，白灰服裝、發光繪圖器。
- 磐：盾衛，深褐重裝、大盾、舊世界軍械感。

## 第一批敵人
- 裂鳴兵：晶化侵蝕的人形守衛。
- 弩手：廢墟掠奪者遠程單位。
- 追光獸：被光源吸引的四足洞窟生物。
- 鳴晶寄生體：附著岩壁的晶體怪。

## 第一批功能物件
- 世界殘圖／測繪核心
- 折疊橋板
- 潮汐鐘
- 鳴晶相位釘
- 蛾火燈
- 軍用權限印記

## 生成策略
先產 style anchor，再產 tileset，再角色，再敵人。每批完成後必須做 consistency review：輪廓、光源方向、像素密度、色盤、尺寸、透視。若任何一項偏離 anchor，重生成，不要把不一致素材混進正式 assets。

## 禁止
- 星級／SSR 視覺框。
- 過度手遊 UI 金光。
- 高清插畫縮小冒充 sprite。
- 角色與地圖透視不一致。
- 素材內含文字或浮水印。

## 對遊戲的接口
正式資產生成後更新 `assets/manifest.json`；遊戲程式優先讀 manifest，沒有素材才回退 procedural placeholder。