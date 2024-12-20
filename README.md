
# scenario-video-assembler

**scenario-video-assembler** は、シナリオファイル（テキストベース）を元に、音声合成（VOICEVOX）・背景画像やキャラクター立ち絵、BGM、SE、字幕、エフェクトなどを組み合わせて、ギャルゲ風の動画を自動生成するためのPythonライブラリおよびCLIツールです。

## 特長

- **シナリオ駆動**：  
  シンプルなテキスト形式のシナリオファイルに、キャラクター、背景、BGM、SE、字幕などの演出指定を記述するだけで動画が自動組み立てされます。

- **デフォルト・カスタマイズ可能な設定**：  
  `characters.yaml` や `system.yaml` により、キャラクターのデフォルトボイス、字幕設定、ピッチ・スピード・ボリュームなどの音声パラメータを管理。指定がない場合はデフォルト値が適用され、必要に応じてシナリオ行ごとに上書き可能です。

- **多彩な演出**：  
  複数のVOICEVOX話者ID対応、表情差分、同時再生・同時エフェクト指定（[PARALLEL]ブロック）、字幕の色・サイズ指定、画像・動画オーバーレイ表示などが可能。

- **CLIコマンド**：  
  `init`コマンドで基本構造のプロジェクトを初期化し、`create`コマンドでシナリオに基づき動画生成を行います。

## インストール

```bash
git clone https://github.com/yourusername/scenario-video-assembler.git
cd scenario-video-assembler
pip install .
```

※ `pyproject.toml` や `setup.py` を用意しておくことで `pip install .` にてローカルインストールが可能です。

## ディレクトリ構成例

```text
scenario-video-assembler/
  ├─ scenario_video_assembler/
  │   ├─ __init__.py
  │   ├─ cli.py
  │   ├─ core/
  │   ├─ utils/
  │   └─ ...
  ├─ data/
  │   ├─ characters/
  │   │   ├─ characters.yaml
  │   │   └─ (キャラ画像等)
  │   ├─ bg/
  │   ├─ bgm/
  │   ├─ se/
  │   ├─ voices/
  │   └─ system.yaml
  ├─ scenarios/
  │   └─ scenario_example.txt
  ├─ tests/
  ├─ README.md
  └─ requirements.txt
```

## 使い方

### 初期化 (init)

`init`コマンドは、必要なデフォルト設定ファイルやディレクトリ構造を自動生成します。  
新規プロジェクト開始時に実行することで、`characters.yaml`や`system.yaml`の雛形、`scenarios`ディレクトリなどを用意できます。

```bash
scenario-video-assembler init --scenario-path ./scenarios/new_scenario.txt --output-dir ./output
```

- `--scenario-path`：雛形となるシナリオファイルの出力先
- `--output-dir`：最終的な生成動画ファイル出力先

### 動画生成 (create)

`create`コマンドは、指定されたシナリオファイルを解析し、VOICEVOXでの音声生成、背景・キャラクター画像の合成、BGM・SE挿入、字幕追加などを行い、一本の動画ファイルを生成します。

```bash
scenario-video-assembler create \
  --scenario ./scenarios/my_scenario.txt \
  --config ./data/system.yaml \
  --characters ./data/characters/characters.yaml \
  --output ./output/final_video.mp4
```

- `--scenario`：シナリオファイルパス
- `--config`：システム共通設定ファイル（`system.yaml`）
- `--characters`：キャラクター定義ファイル
- `--output`：出力動画ファイルパス

### シナリオファイル記述例

```text
[SCENE: 001]
[BG: bg_forest.jpg fadeIn]
[BGM: bgm_ambient_forest start volume=0.8]

[CHAR: hero fadeIn normal center]
hero: 「ここがミッション開始地点か…オペレーター、状況はどうなっている？」

[CHAR: operator fadeIn normal right]
operator: (speed=0.9 volume=0.8)「全てを見通しています、レイヴン。敵拠点はこの先にあります。」

[SE: se_notice.wav]
operator: 「あなたの選択は最適化されています。次は敵拠点へ移動してください。」
```

記述詳細は `characters.yaml` で定義したデフォルト設定や `system.yaml` のシステムデフォルト値を自動参照します。

## 開発

- ローカル環境でのテスト  
  ```bash
  pytest tests
  ```
  
- コード整形・静的解析  
  ```bash
  black scenario_video_assembler
  flake8 scenario_video_assembler
  ```

## ライセンス

本リポジトリは MIT License の下で提供されます。
