# ZXing.Net x64 互換性検証プロジェクト (VS 2022)

## 概要

本プロジェクトは、32bit 環境から Visual Studio 2022 (64bit) への移行に伴い、ZXing.Net ライブラリが 64bit プロセスおよび WinForms 設計器で正常に動作するかを検証するためのサンドボックス環境です。

## 検証の背景

VS 2022 以降、IDE 自体が 64bit 化されたことで、従来の 32bit (x86) 専用 DLL を使用している WinForms 設計器が開けない問題が発生します。本プロジェクトでは、zxing.dll が AnyCPU として正しく動作し、64bit 環境において設計器のクラッシュや実行時のエラーを引き起こさないことを証明しました。

## 環境構成

- **IDE**: Visual Studio 2022
- **ターゲットフレームワーク**: .NET Framework 4.8
- **プラットフォーム**: x64 (検証のため明示的に指定)
- **使用ライブラリ**:
  - zxing.dll (v0.16.6.0)

## プロジェクト構造

```
ZXing_Verification/
├── ZXing_Verification.sln          # ソリューションファイル
├── CLAUDE.md                        # 開発ガイドライン
├── README.md                        # 本ファイル
├── .gitignore                       # Git 除外設定
└── ZXing_Verification/              # プロジェクトディレクトリ
    ├── ZXing_Verification.csproj    # プロジェクトファイル
    ├── App.config                   # アプリケーション構成
    ├── Program.cs                   # エントリポイント
    ├── Form1.cs                     # メインフォーム (QR生成ロジック)
    ├── Form1.Designer.cs            # フォームデザイナコード
    ├── Form1.resx                   # リソースファイル
    ├── Properties/                  # プロパティディレクトリ
    │   ├── AssemblyInfo.cs          # アセンブリ情報
    │   ├── Resources.Designer.cs    # リソース自動生成コード
    │   ├── Resources.resx           # リソース定義
    │   ├── Settings.Designer.cs     # 設定自動生成コード
    │   └── Settings.settings        # アプリケーション設定
    └── lib/                         # ライブラリディレクトリ
        └── zxing.dll                # ZXing.Net バーコードライブラリ
```

## 検証手順

### 1. リポジトリのクローン

```bash
git clone https://github.com/pasysxa/ZXing_Verification.git
cd ZXing_Verification
```

### 2. ソリューションの起動

Visual Studio 2022 で `ZXing_Verification.sln` ファイルを開きます。

### 3. プラットフォームの確認

構成マネージャーで「アクティブなソリューション プラットフォーム」が **x64** になっていることを確認してください。

### 4. 設計器のテスト

`Form1.cs` をダブルクリックし、WinForms デザイナが正常に表示されるか確認します。

### 5. 実行テスト

**F5** キーでデバッグ実行し、テキストを入力して QR コードが正常に生成されるか確認します。

## 検証結果の要約

- **静的解析**: CorFlags により `32BITREQ: 0` (AnyCPU) であることを確認済み。
- **設計器**: VS 2022 の 64bit プロセス下で正常にロード可能。
- **実行時**: x64 プロセスにおいて `BadImageFormatException` を発生させることなく動作。

## 結論

ZXing.Net は 64bit プロジェクトへの移行において安全に使用可能です。

## 機能説明

本アプリケーションは以下の機能を提供します：

- テキストボックスに入力された文字列から QR コードを生成
- 生成された QR コードを 200x200 ピクセルの画像として表示
- ZXing.Net ライブラリの `BarcodeWriter` および `BarcodeReader` クラスの動作確認

## ビルド方法

### MSBuild 使用

```bash
# Debug ビルド (x64)
msbuild ZXing_Verification.sln /p:Configuration=Debug /p:Platform="Any CPU"

# Release ビルド
msbuild ZXing_Verification.sln /p:Configuration=Release /p:Platform="Any CPU"
```

### Visual Studio 使用

1. ソリューションを開く
2. ビルド > ソリューションのビルド (Ctrl+Shift+B)
3. 実行 > デバッグの開始 (F5)

## ライセンス

本検証プロジェクトはサンドボックス環境として作成されました。ZXing.Net ライブラリ自体のライセンスについては、[ZXing.Net 公式リポジトリ](https://github.com/micjahn/ZXing.Net)を参照してください。
