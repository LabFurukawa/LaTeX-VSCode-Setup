# ローカル環境(Ubuntu)で<br>LaTeXを使えるようにする方法

1. 2021版のTeXを入れるために、まず2021版のTeXインストーラーを取得

    `wget http://ftp.math.utah.edu/pub/tex/historic/systems/texlive/2021/install-tl-unx.tar.gz`

2. 展開してディレクトリに入る

    `tar xvf install-tl-unx.tar.gz`

    `cd install-tl-2021*`

    入った後、

    `ls`

    で install-tl があることを確認して(注意：行分けずに実行)

    `sudo ./install-tl -repository http://ftp.math.utah.edu/pub/tex/historic/systems/texlive/2021/tlnet-final`

    Iを押してインストール開始(30-40分くらいかかります)

3. インストール終わったら

    `/usr/local/texlive/2021/bin/x86_64-linux/xelatex --version`

    でtexが入っていることを確認。<br>
    XeTeX 3.141592653-2.6-0.999993 (TeX Live 2021) 的なのが出ればOK

    2021ver. に固定 (注意：行分けずに実行)

    `tlmgr option repository http://ftp.math.utah.edu/pub/tex/historic/systems/texlive/2021/tlnet-final`

4. VSコードを2021ver. Tex 専用PATHをつくる

    `nano ~/code-tex2021.sh`

    作ったファイルの中に<br>
    `export PATH=/usr/local/texlive/2021/bin/x86_64-linux:$PATH
    exec code "$@"`<br>
    を書き込む(ctrl + s)

    nanoを閉じて(ctrl + x)、作ったファイルに実行権限を与える<br>
    `chmod +x ~/code-tex2021.sh`(ターミナルで実行)

5. 卒論のディレクトリ(あらかじめローカルに作っておく)に入って<br>
    `~/code-tex2021.sh .` <br>
    を実行すれば2021Tex用のVSCodeが起動する

6. VSCodeにLaTeX Workhsopの拡張を入れる

    ctrl + shift + p からOpen Settings (JSON) を探して開き、
    下のテキストを{}の中にコピペして保存<br>
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
    "latex-workshop.latex.defaultRecipe": "XeLaTeX 2021 (Legacy)"`

7. `ctrl + alt + b` でPDFが出力されるはず

    多分エラーが出るので、自分の場合`main.tex`の

    \usepackage[dvipdfmx]{graphicx} %画像の挿入のため<br>
    ↓ [dvipdfmx]削除<br>
    \usepackage{graphicx} %画像の挿入のため<br>

    \usepackage{zxjatype} -> 削除
    \usepackage[ipa]{zxjafont}　-> 削除

    でエラー無く出力できました