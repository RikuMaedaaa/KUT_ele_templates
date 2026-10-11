# 便利機能など
CUIやgitなどの操作に慣れている人向けの便利機能の紹介です。
**ローカルのTeXを使用している (OverleafやCloudLaTeXを使用していない) 人向けの**内容です。

## 中間ファイルやPDFの出力先を変更する
`.latexmkrc` に以下の設定を追加すると、`out` ディレクトリ内に中間ファイルや `.pdf` が出力されるようになります。
```
$out_dir = 'out';
```

CloudLaTeXではこの設定が有効になっているとコンパイルエラーが発生するためご注意ください。

## SVGを自動でPDFに変換する
[Inkscape](https://inkscape.org/)がインストールされている状態で .latexmkrc に以下を追加すると、コンパイル時に自動でSVGをPDFへ変換してくれます。
```
add_cus_dep('svg', 'pdf', 0, 'svg2pdf');
sub svg2pdf {
    system("inkscape '$_[0].svg' --export-filename='$_[0].pdf'  --export-dpi=100 --export-overwrite");
}
```
このとき、PowerPoint等でSVGを作成した際に使用したフォントがInkscape側で利用可能になっていないと、文字や影などのレイアウトが崩れます。
特に、WindowsのPowerPointで作成したSVGをWSL上で自動変換・コンパイルする場合はご注意ください。

Windows11とUbuntu 24.04で動作確認済みです。

## WSLに TeX Live をインストールする
Windows上で以下のコマンドを実行し、WSLと Ubuntu のインストールを行います。
```
wsl --install Ubuntu-24.04
```
その後、WSL（Ubuntu-24.04）のシェルで以下を実行して TeX Live をインストールします。
```
sudo apt install texlive-full -yU
```

「SVGの自動PDF変換」機能を使いたい場合は、Inkscapeもインストールします。
```
sudo apt install inkscape -yU
```
インストール後、`/etc/fonts/local.conf` を作成して以下を記入します。
```
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
    <dir>/mnt/c/Windows/Fonts</dir>
</fontconfig>
```
この手順により、Windows側のフォントがWSL上（Inkscape等）でも利用可能になります。
