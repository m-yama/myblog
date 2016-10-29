.. post:: Oct 29, 2016
   :tags: python, pip
   :category: blog


(3) pipでインストール中にエラーが出た話
=======================================

pip で色々インストールしている最中に、以下のようなエラーが発生。

``error: Microsoft Visual C++ 9.0 is required (Unable to find vcvarsall.bat).`` 

マイクロソフトのダウンロードセンターにこんなものがあり、
入れたらエラーが解消されました。

`Microsoft Visual C++ Compiler for Python 2.7 <https://www.microsoft.com/en-us/download/details.aspx?id=44266>`_

公式にこんなものが配布されてるんですね。

