# ローカル環境(Ubuntu)でLaTeXを使えるようにする方法

## 概要
※ 卒論テンプレートが TeX Live 2021 を前提としているため、
2021 環境を構築します。


テンプレートはこちら：
[thesis-template.zip](thesis-template.zip)


Ubuntu 環境で TeX Live 2021 を使用し、
卒論用 LaTeX 環境を構築する手順です。
VSCode から PDF のビルドが可能になります。



## 1. 2021版のTeXインストーラーで2021版のTeXをインストール

1. 以下をターミナルで実行し、インストーラーを取得

    ```bash
    wget http://ftp.math.utah.edu/pub/tex/historic/systems/texlive/2021/install-tl-unx.tar.gz
    ```

2. 取得したファイルを展開してディレクトリに入る

    ```bash
    tar xvf install-tl-unx.tar.gz

    cd install-tl-2021*
    ```

3. 入った後、`ls`で `install-tl`があることを確認して、TeX(2021)をインストール

    ```bash
    sudo ./install-tl -repository http://ftp.math.utah.edu/pub/tex/historic/systems/texlive/2021/tlnet-final
    ```

    `I`を押してインストール開始(30-40分くらいかかります)

## 2. インストール確認と ver. の固定

1. インストールの確認

    TeX が入っていることを確認
    ```bash
    /usr/local/texlive/2021/bin/x86_64-linux/xelatex --version
    ```
    
    以下のような表示がされればインストール成功

    ```bash
    XeTeX 3.141592653-2.6-0.999993 (TeX Live 2021) 
    ```

2. 2021ver. に固定

    ```bash
    tlmgr option repository http://ftp.math.utah.edu/pub/tex/historic/systems/texlive/2021/tlnet-final
    ```

## 3. VSCode 用 PATH の作成

1. PATHの作成

    ```bash
    nano ~/code-tex2021.sh
    ```

    作ったファイルの中に

    ```bash
    export PATH=/usr/local/texlive/2021/bin/x86_64-linux:$PATH
    exec code "$@"
    ```
    を書き込む(`ctrl + o → Enter`)

    nanoを閉じて(`ctrl + x`)、作ったファイルに実行権限を与える

    ```bash
    chmod +x ~/code-tex2021.sh
    ```

2. 卒論のディレクトリ(あらかじめローカルに作っておく)に入って

    ```bash
    ~/code-tex2021.sh .
    ```
    
    VSCode が起動すれば成功

## 4. VSCode の設定

VSCodeにLaTeX Workshopの拡張を入れる

`ctrl + shift + p` から`Open Settings (JSON)` を探して開き、
下のテキストをコピペして保存
> ⚠️ `{}` の外には貼らないでください


```json
"latex-workshop.latex.autoBuild.run": "never",
"latex-workshop.latex.tools": [
    {
    "name": "xelatex-2021",
    "command": "xelatex",
    "args": [
        "-interaction=nonstopmode",
        "-synctex=1",
        "%DOC%"
    ]
    }
],
"latex-workshop.latex.recipes": [
    {
    "name": "XeLaTeX 2021 (Legacy)",
    "tools": ["xelatex-2021"]
    }
],
"latex-workshop.latex.defaultRecipe": "XeLaTeX 2021 (Legacy)"
```

## 5. PDF のビルド確認

`ctrl + alt + b` でPDFが出力されれば成功です

## 6. トラブルシュート(調整予定)

PDFで日本語が表示されない場合
`main.tex`で

```latex
\usepackage[dvipdfmx]{graphicx} %画像の挿入のため

↓

\usepackage{graphicx} %画像の挿入のため
```

に変更。また、

```latex
\usepackage{zxjatype} 
\usepackage[ipa]{zxjafont}
```

を削除

でエラー無く出力できました