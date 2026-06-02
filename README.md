# みんなで勝ち時ラジオ（運用マニュアル）

株式会社メディアエイド 社内向け音声配信サイト。
録音(mp3)を GitHub Pages に置き、RSSフィード(podcast.xml)を配信。
社員は YouTube Music にフィードURLを追加して聴く。

## 公開URL
- 配信サイト: https://mediaaidradio-lab.github.io/kachidoki-radio/
- フィードURL（社員に配るもの）: https://mediaaidradio-lab.github.io/kachidoki-radio/podcast.xml

※ `mediaaidradio-lab` は実際のGitHubユーザー名に置き換える。

## ファイル構成
- `podcast.xml` … RSSフィード。エピソードはここに `<item>` を追記
- `index.html` … 社員向けインストール案内（STEP 1〜10）
- `cover.png` … 番組カバー画像（正方形・1400〜3000px）
- `episodeNN.mp3` … 音声ファイル（連番）
- `step01〜09.png` / `youtube-music.png` … 案内ページ用の画像

---

## 新エピソードの追加手順

### 方法A：Claude Code に任せる（おすすめ）
Claude Code に下記を貼るだけ:

```
新しいラジオ回を追加して公開してください。
録音ファイル: ~/Downloads/今日の録音.m4a
タイトル: 第NN回｜今日のテーマ

手順:
1. m4aならmp3に変換
2. episodeNN.mp3（連番）として配置
3. podcast.xml に <item> を追加（title / enclosure url / length=正確なバイトサイズ / pubDate=今日 / guid=kachidoki-ep-NNN）
4. git commit & push
5. 反映URLを確認して教えて
```

### 方法B：手動で行う場合
1. 音声を `episodeNN.mp3` としてこのフォルダに置く（m4aの場合はmp3に変換）
2. ファイルのバイトサイズを確認（例: `stat -f%z episodeNN.mp3`）
3. `podcast.xml` の `</channel>` の直前に `<item>` を追記
   - `title` / `enclosure url` / `length`（=バイトサイズ）/ `pubDate` / `guid`
4. 保存して commit & push
   ```
   git add episodeNN.mp3 podcast.xml
   git commit -m "Add Episode NN"
   git push origin main
   ```

---

## つまずきポイント
1. **`length` はバイト数を正確に**。ファイルサイズと1バイトでもズレると一部アプリで再生が壊れる。
2. **YouTube Musicは反映が遅い**。pushしてもアプリに出るまで数時間〜半日かかる（Google側の巡回待ち。サイト側は正常）。
3. **配るURLは1つに統一**。社員に渡すフィードURLは、実際に公開されている `…/podcast.xml` であることを必ず確認。

## 注意（公開範囲）
GitHub Pages（無料）はリポジトリ公開のため、音声はURLを知っていれば誰でもアクセス可能。
社外秘の内容は扱わない前提で運用すること。
