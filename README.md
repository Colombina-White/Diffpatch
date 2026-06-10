# WhiteUpd

[繁中](README_CHT.md) | [简中](README_CHS.md) | EN

Reference: [HappyGenyuanImsactUpdate](https://github.com/YYHEggEgg/HappyGenyuanImsactUpdate)

`WhiteUpd` is a command-line tool for creating game update packages. It supports two output modes:

- `hdiff`: generates `hdifffiles.txt`, `.hdiff` files, and falls back to full-file copies when needed.
- `ldiff`: generates a Sophon-style `manifest` and `ldiff\chunk` payload.

## Basic Usage

```powershell
WhiteUpd.exe hdiff -from 4.4.0 C:\HSR4.4 -to 4.5.0 D:\bak -output_to D:\hdiff --only-include-pkg-defined-files
```

```powershell
WhiteUpd.exe ldiff -from 4.4.0 C:\HSR4.4 -to 4.5.0 D:\bak -output_to D:\ldiff --only-include-pkg-defined-files
```

## ldiff Block Inference

`ldiff` builds a chunk index for `.block` files by default. This helps detect old files with different names but similar content, which can reduce the final patch size.

Default options:

```text
--moved-block-infer-method index
--moved-block-candidates 16
--moved-block-chunk-pool 96
--moved-block-chunk-size 131072
--moved-block-index-min-score 2
--moved-block-size-window 0.25
--moved-block-min-saving-ratio 0.85
```

Increase candidate count:

```powershell
WhiteUpd.exe ldiff -from 4.4.0 C:\HSR4.4 -to 4.5.0 D:\bak -output_to D:\ldiff --only-include-pkg-defined-files --moved-block-candidates 32
```

Use 64KB chunks:

```powershell
WhiteUpd.exe ldiff -from 4.4.0 C:\HSR4.4 -to 4.5.0 D:\bak -output_to D:\ldiff --only-include-pkg-defined-files --moved-block-chunk-size 65536
```

Disable moved-block inference:

```powershell
WhiteUpd.exe ldiff -from 4.4.0 C:\HSR4.4 -to 4.5.0 D:\bak -output_to D:\ldiff --only-include-pkg-defined-files --no-infer-moved-blocks
```

## Output Name

```text
<prefix>_<fromVersion>_<toVersion>_<mode>_<random>.zip
```

Examples:

```text
game_4.4.0_4.5.0_hdiff_xxxxx.zip
game_4.4.0_4.5.0_ldiff_xxxxx.zip
```

## Required Files

- `WhiteUpd.exe`
- `WhiteUpd.dll`
- `WhiteUpd.deps.json`
- `WhiteUpd.runtimeconfig.json`
- `hdiffz.exe`
- `7z.exe`
- `7z.dll`
- `zstd.exe`

## Arguments

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
