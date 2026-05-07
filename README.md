# Multilingual Audio Dataset

这个仓库用于发布多语种 ASR 测试音频数据。音频数据不直接提交到 Git 仓库中，而是放在 GitHub Release 里，按语种分别打包，方便单独下载。

Release 地址：

```text
https://github.com/mixi545/audio/releases/tag/audio-dataset-20260507
```

## 数据结构

每个语种是一个独立的 zip 文件，例如：

```text
ja.zip
ko.zip
zh-CN.zip
```

每个 zip 解压后结构一致：

```text
lab.txt
audio/
  ja_001.wav
  ja_002.wav
  ...
```

`lab.txt` 格式：

```text
音频ID<TAB>文本
```

示例：

```text
ja_001	私は静かで綺麗な所に住みたいです。
```

音频文件为 WAV，文件名和 `lab.txt` 第一列对应。

## 下载单个语种

### 使用浏览器

打开 Release 页面：

```text
https://github.com/mixi545/audio/releases/tag/audio-dataset-20260507
```

在 Assets 中找到需要的语种包，例如 `ja.zip`，点击下载即可。

### 使用 GitHub CLI

安装并登录 GitHub CLI 后，可以只下载某一个语种。

下载日语：

```bash
gh release download audio-dataset-20260507 \
  --repo mixi545/audio \
  --pattern 'ja.zip' \
  --dir ./downloaded_audio
```

下载韩语：

```bash
gh release download audio-dataset-20260507 \
  --repo mixi545/audio \
  --pattern 'ko.zip' \
  --dir ./downloaded_audio
```

解压：

```bash
unzip ./downloaded_audio/ja.zip -d ./downloaded_audio/ja
```

### 使用 wget 或 curl

下载日语：

```bash
wget -O ja.zip https://github.com/mixi545/audio/releases/download/audio-dataset-20260507/ja.zip
```

或：

```bash
curl -L -o ja.zip https://github.com/mixi545/audio/releases/download/audio-dataset-20260507/ja.zip
```

下载其它语种时，把 URL 最后的 `ja.zip` 改成对应文件名即可。

## 下载全部语种

使用 GitHub CLI 下载全部 Release assets：

```bash
gh release download audio-dataset-20260507 \
  --repo mixi545/audio \
  --dir ./downloaded_audio
```

批量解压：

```bash
mkdir -p ./downloaded_audio/unzipped
for f in ./downloaded_audio/*.zip; do
  name=$(basename "$f" .zip)
  unzip "$f" -d "./downloaded_audio/unzipped/$name"
done
```

## 当前语种包

当前 Release 按语种提供 51 个 zip：

```text
ar az bg bn br ca cnh cs cy de dv el es eu fa fr fy-NL gl hi ia id it ja ka ko lt mhr mn mrj nl or pl ps pt rm-sursilv ro ru rw sk ta th tok tr ug ur uz vi yue zh-CN zh-HK zh-TW
```

大多数语种为 `500` 条音频和 `500` 行标签。已知非 500 条语种：

```text
az: audio=29, lab=29
ko: audio=228, lab=228
ps: audio=195, lab=195
```

`ja.zip` 中的 `lab.txt` 已按提供的副本文件修正后再打包。

## 上传和维护

新增或更新 Release asset 时，推荐继续使用一个语种一个 zip 的方式：

```bash
gh release upload audio-dataset-20260507 ./ja.zip \
  --repo mixi545/audio \
  --clobber
```

创建新版本 Release：

```bash
gh release create audio-dataset-YYYYMMDD ./*.zip \
  --repo mixi545/audio \
  --title audio-dataset-YYYYMMDD \
  --notes "Multilingual audio dataset packaged by language."
```
