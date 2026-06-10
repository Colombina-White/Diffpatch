# WhiteUpd

`WhiteUpd` 用來建立遊戲版本更新包，支援兩種模式：

- `hdiff`: 產生傳統 `hdifffiles.txt` + `.hdiff` / copy 檔案。
- `ldiff`: 產生 Sophon 類型的 `manifest` + `ldiff\chunk`。

## 基本用法

```powershell
WhiteUpd.exe hdiff -from 4.4.0 C:\HSR4.4 -to 4.5.0 D:\bak -output_to D:\hdiff --only-include-pkg-defined-files
```

```powershell
WhiteUpd.exe ldiff -from 4.4.0 C:\HSR4.4 -to 4.5.0 D:\bak -output_to D:\ldiff --only-include-pkg-defined-files
```

第一個參數是模式。沒填時預設是 `hdiff`。

## ldiff 推測 block 來源

`ldiff` 模式預設會嘗試推測不同檔名的 `.block` 來源。

可調參數：

```text
--no-infer-moved-blocks
--moved-block-candidates <count>
--moved-block-size-window <ratio>
--moved-block-min-saving-ratio <ratio>
```

預設值：

```text
--moved-block-candidates 16
--moved-block-size-window 0.08
--moved-block-min-saving-ratio 0.85
```

如果你想測更多候選，可能會變小，但會更慢：

```powershell
WhiteUpd.exe ldiff -from 4.4.0 C:\HSR4.4 -to 4.5.0 D:\bak -output_to D:\ldiff --only-include-pkg-defined-files --moved-block-candidates 32
```

如果想關掉推測：

```powershell
WhiteUpd.exe ldiff -from 4.4.0 C:\HSR4.4 -to 4.5.0 D:\bak -output_to D:\ldiff --only-include-pkg-defined-files --no-infer-moved-blocks
```

## 輸出檔名

```text
<prefix>_<fromVersion>_<toVersion>_<mode>_<random>.zip
```

例如：

```text
game_4.4.0_4.5.0_hdiff_xxxxx.zip
game_4.4.0_4.5.0_ldiff_xxxxx.zip
```

`-p` 可以改 prefix：

```powershell
WhiteUpd.exe ldiff -from 4.4.0 C:\HSR4.4V1 -to 4.5.0 D:\bak -output_to D:\ldiff -p game --only-include-pkg-defined-files
```

## 需要的工具

`publish` 資料夾內需要：

- `WhiteUpd.exe`
- `hdiffz.exe`
- `7z.exe`
- `7z.dll`
- `zstd.exe`

工具不在 exe 同目錄時可以手動指定：

```powershell
WhiteUpd.exe ldiff ... --hdiffz D:\tools\hdiffz.exe --7z D:\tools\7z.exe --zstd D:\tools\zstd.exe
```

## 常用參數

```text
-from <versionFrom> <source_directory>
-to <versionTo> <target_directory>
-output_to <output_zip_directory>
-p <prefix>
--only-include-pkg-defined-files
--include-audios
--decrypt-command "decrypt.exe {input} {output}"
--hdiffz <path>
--7z <path>
--zstd <path>
--keep-temp
```
