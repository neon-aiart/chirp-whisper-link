# 🐦 Chirp Whisper Link v4.8  

<img src="https://raw.githubusercontent.com/neon-aiart/chirp-whisper-link/main/00228-434673803.png" style="height: 200px; width: 200px; object-fit: contain;" align="right" alt="thumbnail" />  

ローカルファイルやGoogleDriveから、Whisperを使って自動で文字起こしするシンプルなColabスクリプトです  
A simple Google Colab script to transcribe audio from local files or GoogleDrive using Whisper.  

⭐ [スター](https://github.com/neon-aiart/chirp-whisper-link/)をポチッとお願いします✨ (Please hit the [Star] button!)

<br clear="right">  

---

## ✨ 特徴 / Features  

- **デュアルモード:** ローカルファイルとGoogleDriveに対応  
  **Dual Mode:** Support for both local file uploads and GoogleDrive.  
- **自動ダウンロード:** 文字起こし結果を自動的に.txt形式で保存・ダウンロード  
  **Auto-Download:** Automatically saves the transcription as a `.txt` file to your computer.  
- **スマートロード:** モデルがロード済みかを自動判定し、待ち時間を短縮  
  **Smart Loading:** Detects if the model is already loaded to save time.  
- **完了通知音:** 処理が終わると「きらめき音」でお知らせ  
  **Notification Sound:** Plays a "sparkle" sound when the task is complete.  

---

## 🚀 使い方 / How to Use  

下のボタンを押してGoogle Colabで実行してください  
Click the button below to open the script in Google Colab and start transcribing!  

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/neon-aiart/chirp-whisper-link/blob/main/chirp-whisper-link%20v4.8.ipynb)  

1. **ランタイムのタイプを変更 / Change Runtime Type**  
  上部メニューの「ランタイム」→「ランタイムのタイプを変更」からハードウェアを選択します  
  (Go to "Runtime" -> "Change runtime type" in the top menu.)  
   * **T4 GPU**: < おすすめ > 高速に処理できます (Recommended for high-speed processing.)  
   * **CPU**: 時間がかかってもいい場合や、GPU枠を節約したい時 (Use this to save GPU quota if you don't mind it taking longer.)  

2. **ランタイムに接続 / Connect to Runtime**  
  右上の「接続」をクリックして準備します (Click "Connect" in the top right corner.)  

3. **モードの選択 / Select Mode**  
  **mode** を設定します: パソコン内のファイルなら `Upload` 、GoogleDriveなら `GoogleDrive`を選択  
  (Set the **mode**: select `Upload` for local files or `GoogleDrive` for GoogleDrive.)  

4. **フォルダー名を入力 (GoogleDriveモード) / Enter Folder Name (for GoogleDrive Mode)**  
  **drive_folder** にフォルダー名を入力します (Enter the folder name in the **drive_folder** field.)  

5. **再生ボタンを押して実行 / Click the Play Button**  
  セルの左側にある再生ボタンをクリックして実行します (Click the Play button on the left side of the cell to start.)  

6. **ファイルのアップロード (Uploadモードの場合) / Upload File (for Upload Mode)**  
  途中で「ファイル選択」ボタンが表示されるので、ファイルを選択してください  
  (When the "Choose Files" button appears, select your audio file.)  

7. **続けて実行する場合 / To Continue**  
  手順3に戻り、設定を変更して再度再生ボタンを押します  
  (Go back to step 3, change settings, and click the Play button again.)  

8. **終わったら接続解除 (ゼッタイ！) / Disconnect (Crucial!)**  
  **「ランタイムを接続解除して削除」** を必ず行ってください  
  (Always select **"Disconnect and delete runtime"** when finished.)  

<details>
<summary><b>📸 スクリーンショット / Interface Screenshot</b></summary>

<p align="center">
  <img src="chirp-whisper-link-screenshot.png" style="width: 720px; object-fit:" alt="Interface Screenshot">
</p>

</details>  

---

### ⚙️ 設定の詳細 / Parameter Details  

* **model_type**:  
  * `turbo`: 早くしてほしい時に (When you want it fast)  
  * `large-v3`: ガンバってほしい時に (When you want the best accuracy)  

* **initial_prompt**:  
  どんな内容か書くと、専門用語などの誤字が減りやすくなります  
  (Helps the AI recognize technical terms and specific context to improve accuracy.)  

* **mode**:  
  `Upload`: パソコン内の音声・動画ファイルを読み込む (Process local files)  
    &emsp;※ `drive_folder`は無視されます (`drive_folder` is ignored)  
  `GoogleDrive`: GoogleDriveにあるファイルから音声を抽出 (Extract from GoogleDrive)  
    &emsp;※ `execute_file_exists`は無視されます (`execute_file_exists` is ignored)  
  `YouTube`: ※ 現在、機能停止中です（Currently unavailable）  

* **drive_folder**:  
  GoogleDriveのフォルダ (GoogleDrive folder)  
    &emsp;※ `mode`が`Upload`のときは無視されます (`mode: Upload` ignores this)  

* **execute_file_exists**:  
  * **ON**: フォルダ内に以前のファイルが残っている場合、**アップロードをスキップして最新のファイルを再利用**します (Reuses the latest file in the folder, **skipping the upload/download process**.)  
  * **OFF**: 新しくファイルをアップロードします (Uploads a local file.)  

  &emsp;※ `mode`が`GoogleDrive`のときは無視されます (`mode: GoogleDrive` ignores this)  

* **records_text_download**:  
  議事録もダウンロード (Toggle to download timestamped transcript (.txt))  

### 🚀 効率化機能：既存ファイルの再利用 / Optimization: Reusing Existing Files  

アップロード・ダウンロード済みの最新ファイルを再利用することで、パラメータ調整時の待ち時間を大幅に短縮できます  
Reuse the most recently uploaded or downloaded file to significantly reduce wait times during parameter tuning.  

#### `mode`を`Upload`にする (switching `mode` to `Upload`)  

* **通常(Standard)**: `File Upload (60s)` + `Whisper (120s)` = **180sec**  
* **再利用モード(Reuse)**: `Whisper (120s)` only = **120sec (33% OFF!)**  

---

## ⚠️ YouTubeモードの提供一時休止について  

現在、YouTube側のセキュリティ強化（Bot検知やPO Tokenの導入）により、外部ツールからの直接ダウンロードが非常に不安定になっています。  

ユーザーの皆様のアカウントの安全を第一に考え、本ツールでは**YouTube URLによる直接指定機能を一時停止（Shelved）** しています。  

現在は以下の2つのモードを推奨しています：  

- **Uploadモード**: ローカルにある音声ファイルを直接アップロードして利用
- **GoogleDriveモード**: GoogleDrive内の指定フォルダ（デフォルトは `Whisper`）にあるファイルを利用  

YouTubeの音声を文字起こししたい場合は、あらかじめご自身で音声ファイルを準備し、上記いずれかのモードでご利用ください。  

## ⚠️ Regarding the Temporary Suspension of YouTube Mode  

Due to enhanced YouTube security measures (Bot detection, PO Tokens, etc.), direct downloads via external tools are currently unstable.  

To ensure account safety, the **YouTube URL input functionality is currently suspended (Shelved)**.  

We recommend using one of the following modes:  

- **Upload Mode**: Upload audio files directly from your local machine.  
- **GoogleDrive Mode**: Use files stored in a specific GoogleDrive folder (default: `Whisper`).  

If you wish to transcribe YouTube content, please prepare the audio file in advance and use the "Upload" or "GoogleDrive" mode.  

---

## ⚠️ 注意事項 / Important  

ファイルがダウンロードされたら、必ず手動で **「ランタイムを接続解除」** してください  
接続したまま放置すると、無料枠のGPU時間がすぐになくなってしまいます  
**Please remember to "Disconnect and delete runtime" manually after use.**  
Leaving it connected will exhaust your remaining GPU time.  

---

## 📝 更新履歴 (Changelog)  

### [v4.8](https://colab.research.google.com/github/neon-aiart/chirp-whisper-link/blob/main/chirp-whisper-link%20v4.8.ipynb) (Current Release)  

✅ GoogleDriveからファイル選択を仮実装  
✅ 長いファイル名でエラーになって保存できなかったのを修正  

### [v4.7](https://colab.research.google.com/github/neon-aiart/chirp-whisper-link/blob/main/chirp-whisper-link%20v4.7.ipynb)  

✅ condition_on_previous_textがなぜかFalseのままだったのでTrueに修正  
✅ YouTubeモードを棚上げ (Shelved)  

### [v4.6](https://colab.research.google.com/github/neon-aiart/chirp-whisper-link/blob/main/chirp-whisper-link%20v4.6.ipynb)  

☑️ YouTubeアクセスにリトライを追加  

### v4.5  

✅ 再利用可能に (execute_file_existsを追加)  
☑️ languageをNoneに変更 (自動判定に任せる)  

### v4.4 (UnReleased)  

✅ 精度を改善  
☑️ condition_on_previous_textを削除 (trueで固定)  

### v4.3  

✅ Faster-Whisperに変更  
✅ 議事録のダウンロードをON/OFFに変更  

### v4.2  

✅ txtを議事録に変更  
✅ ダウンロードするファイルにsrtを追加  
☑️ 軽微な修正  

### v4.1  

✅ 初リリース  

---

## 🛡️ ライセンスについて (License)  

このノートブックのソースコードは、ねおんが著作権を保有しています  
The source code for this notebook is copyrighted by Neon.  

* **ライセンス / License**: **[PolyForm Noncommercial 1.0.0](https://polyformproject.org/licenses/noncommercial/1.0.0/)** です（LICENSEファイルをご参照ください）  
  Licensed under PolyForm Noncommercial 1.0.0. (Please refer to the LICENSE file for details.)  
* **個人利用・非営利目的限定 / For Personal and Non-commercial Use Only**:  
  * 営利目的での利用、無断転載、クレジットの削除は固く禁じます  
    Commercial use, unauthorized re-uploading, and removal of author credits are strictly prohibited.  
* **再配布について / About Redistribution**:  
  * 本プロジェクトを改変・配布（フォーク）する場合は、必ず元の作者名（ねおん）およびクレジット表記を維持してください  
    If you modify or redistribute (fork) this project, you MUST retain the original author's name (Neon) and all credit notations.  

※ ご利用は自己責任でお願いします（悪用できるようなものではないですが、念のため！）  

---

## ⚠️ セキュリティ警告 / Security Warning  

🚨 **重要：公式配布について / IMPORTANT: Official Distribution**  
当プロジェクトの公式ノートブックは、**GitHub** でのみ公開しています。  
The official notebook for this project is ONLY available on **GitHub**.  

🚨 **偽物に注意 / Beware of Fakes**  

* 他サイト等で `.zip`, `.exe`, `.cmd` 形式で配布されているものはすべて**偽物**です。  
  これらには**ウイルスやマルウェア**が含まれていることが確認されており、非常に危険です。  
* Any distribution in `.zip`, `.exe`, `.cmd` formats on other sites is **FAKE**.  
  These have been confirmed to contain **VIRUSES or MALWARE**.  

### ⚖️ 法的措置と通報について / Legal Action & Abuse Reports  

当プロジェクトの制作物に対する無断転載が確認されたため、過去に **DMCA Take-down通知** を送付しています。  
また、マルウェアを配布する悪質なサイトについては、順次 **各機関へ通報 (Malware / Abuse Report)** を行っています。  
We have filed **DMCA Take-down notices** against unauthorized re-uploads of my projects.  
Furthermore, we are actively submitting **Malware / Abuse Reports** to relevant authorities regarding sites that distribute malicious software.  

---

## 開発者 (Author)  

**ねおん (Neon)**  
<pre>
<img src="https://www.google.com/s2/favicons?domain=bsky.app&size=16" alt="Bluesky icon"> Bluesky       :<a href="https://bsky.app/profile/neon-ai.art/">https://bsky.app/profile/neon-ai.art/</a>
<img src="https://www.google.com/s2/favicons?domain=github.com&size=16" alt="GitHub icon"> GitHub        :<a href="https://github.com/neon-aiart/">https://github.com/neon-aiart/</a>
<img src="https://neon-aiart.github.io/favicon.ico" alt="neon-aiart icon" width="16" height="16"> GitHub Pages  :<a href="https://neon-aiart.github.io/">https://neon-aiart.github.io/</a>
<img src="https://www.google.com/s2/favicons?domain=greasyfork.org&size=16" alt="Greasy Fork icon"> Greasy Fork   :<a href="https://greasyfork.org/ja/users/1494762/">https://greasyfork.org/ja/users/1494762/</a>
<img src="https://www.google.com/s2/favicons?domain=zenn.dev&size=16" alt="Sizu icon"> Zenn Dev      :<a href="https://zenn.dev/neon_aiart/">https://zenn.dev/neon_aiart/</a>
<img src="https://www.google.com/s2/favicons?domain=sizu.me&size=16" alt="Sizu icon"> Sizu Diary    :<a href="https://sizu.me/neon_aiart/">https://sizu.me/neon_aiart/</a>
<img src="https://www.google.com/s2/favicons?domain=ofuse.me&size=16" alt="Ofuse icon"> Ofuse         :<a href="https://ofuse.me/neon/">https://ofuse.me/neon/</a>
<img src="https://www.google.com/s2/favicons?domain=www.chichi-pui.com&size=16" alt="chichi-pui icon"> chichi-pui    :<a href="https://www.chichi-pui.com/users/neon/">https://www.chichi-pui.com/users/neon/</a>
<img src="https://www.google.com/s2/favicons?domain=iromirai.jp&size=16" alt="iromirai icon"> iromirai      :<a href="https://iromirai.jp/creators/neon/">https://iromirai.jp/creators/neon/</a>
<img src="https://www.google.com/s2/favicons?domain=www.days-ai.com&size=16" alt="DaysAI icon"> DaysAI        :<a href="https://www.days-ai.com/users/lxeJbaVeYBCUx11QXOee/">https://www.days-ai.com/users/lxeJbaVeYBCUx11QXOee/</a>
</pre>

---
