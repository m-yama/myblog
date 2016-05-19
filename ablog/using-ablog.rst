.. post:: May 15, 2016
   :tags: ablog
   :category: blog


ABlogを使ってみる
====================

Sphinxでブログを書いてみたいと思い、Googleで検索したら *Ablog* というのが見つかったので使ってみる。


いきなりつまづく
--------------------

最初、Windowsで試したところ、 ``ablog start`` で生成されたファイルが *shift_jis* で作られていたため、 ``ablog build`` すると `UnicodeDecodeError` になって進まない。

open関数を使っているところを、

.. code:: python

   open(path, encode='utf-8')

というふうに書き換えれば動くが、ソースをいじると後々面倒なのでとりあえず Mac で書くようにした。

.. note::

   最終的には Mac と Windows 両方で使えるようにしたい。


解決
----

そういえばPython3から、open関数の引数でエンコードの指定をしないと開けないとか、何かで見た気がする。
WindowsではPython2.7で動かすようにしたら、上記のエラーは出なくなった。


テーマ変更がうまくいかない
------------------------------

テーマを変えようとしたら、 ``ablog build`` すると以下のエラーメッセージが出てビルドできない。
なんか面倒そうなので、とりあえず保留。

.. code:: python

   Exception occurred:
     File "c:\python27\lib\site-packages\sphinx\jinja2glue.py", line 159, in get_source
       raise TemplateNotFound(template)
   TemplateNotFound: about.html
