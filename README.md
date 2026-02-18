# ローカル環境(Ubuntu)で<br>LaTeXを使えるようにする方法

1. 2021版のTeXを入れるために、まず2021版のTeXインストーラーを取得

    `wget http://ftp.math.utah.edu/pub/tex/historic/systems/texlive/2021/install-tl-unx.tar.gz`

2. 展開してディレクトリに入る

    `tar xvf install-tl-unx.tar.gz`

    `cd install-tl-2021*`

    入った後、

    `ls`

    で install-tl があることを確認して(注意：2行分けずに実行)

    `sudo ./install-tl -repository http://ftp.math.utah.edu/pub/tex/historic/systems/texlive/2021/tlnet-final`

    Iを押してインストール開始

    