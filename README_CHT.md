# WhiteUpd

繁中 | [简中](README_CHS.md) | [EN](README.md)

參考：[HappyGenyuanImsactUpdate](https://github.com/YYHEggEgg/HappyGenyuanImsactUpdate)

`WhiteUpd` 是用來製作遊戲更新包的命令列工具，支援兩種輸出模式：

- `hdiff`：產生 `hdifffiles.txt`、`.hdiff`，必要時直接複製完整檔案。
- `ldiff`：產生 Sophon 類型的 `manifest` 與 `ldiff\chunk`。

## 基本用法

```powershell
WhiteUpd.exe hdiff -from 4.4.0 C:\HSR4.4 -to 4.5.0 D:\bak -output_to D:\hdiff --only-include-pkg-defined-files
```

```powershell
WhiteUpd.exe ldiff -from 4.4.0 C:\HSR4.4 -to 4.5.0 D:\bak -output_to D:\ldiff --only-include-pkg-defined-files
```

## ldiff Block 推測

`ldiff` 預設會對 `.block` 檔建立 chunk index，用來推測「檔名不同但內容相近」的舊檔，讓輸出的 patch 變小。

預設參數：

```text
--moved-block-infer-method index
--moved-block-candidates 16
--moved-block-chunk-pool 96
--moved-block-chunk-size 131072
--moved-block-index-min-score 2
--moved-block-size-window 0.25
--moved-block-min-saving-ratio 0.85
```

提高候選數：

```powershell
WhiteUpd.exe ldiff -from 4.4.0 C:\HSR4.4 -to 4.5.0 D:\bak -output_to D:\ldiff --only-include-pkg-defined-files --moved-block-candidates 32
```

改用 64KB chunk：

```powershell
WhiteUpd.exe ldiff -from 4.4.0 C:\HSR4.4 -to 4.5.0 D:\bak -output_to D:\ldiff --only-include-pkg-defined-files --moved-block-chunk-size 65536
```

關閉搬移 block 推測：

```powershell
WhiteUpd.exe ldiff -from 4.4.0 C:\HSR4.4 -to 4.5.0 D:\bak -output_to D:\ldiff --only-include-pkg-defined-files --no-infer-moved-blocks
```

## 輸出檔名

```text
<prefix>_<fromVersion>_<toVersion>_<mode>_<random>.zip
```

範例：

```text
game_4.4.0_4.5.0_hdiff_xxxxx.zip
game_4.4.0_4.5.0_ldiff_xxxxx.zip
```

## 需要的檔案

`publish` 資料夾內建議附上：

- `WhiteUpd.exe`
- `WhiteUpd.dll`
- `WhiteUpd.deps.json`
- `WhiteUpd.runtimeconfig.json`
- `hdiffz.exe`
- `7z.exe`
- `7z.dll`
- `zstd.exe`
- `README.md`
- `README_CHT.md`
- `README_CHS.md`

不要公開：

- `WhiteUpd.pdb`
- `WhiteUpd.protected.dll.map`

## 參數

```text
whiteupd [hdiff|ldiff]
  -from <versionFrom> <source_directory>
  -to <versionTo> <target_directory>
  -output_to <output_zip_directory>
  [-p <prefix>]
  [--only-include-pkg-defined-files [--include-audios]]
  [--decrypt-command "decrypt.exe {input} {output}"]
  [--hdiffz <path>] [--7z <path>] [--zstd <path>]
  [--no-infer-moved-blocks]
  [--moved-block-infer-method index|chunk|size]
  [--moved-block-candidates <count>]
  [--moved-block-chunk-pool <count>]
  [--moved-block-chunk-size <bytes>]
  [--moved-block-index-min-score <count>]
  [--moved-block-size-window <ratio>]
  [--moved-block-min-saving-ratio <ratio>]
  [--keep-temp]
```
