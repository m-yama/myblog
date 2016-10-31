.. post:: May 15, 2016
   :tags: ablog, sphinx
   :category: blog


ABlogを使ってSphinxでブログを書いてみる
=======================================

Sphinxでブログを書いてみたいと思い、Googleで検索したら `ABlog for Sphinx <http://ablog.readthedocs.io/>`_ というのが見つかったので、使ってみる。


いきなりつまづく
----------------

最初、Windowsで試したところ、 ``ablog start`` で生成されたファイルが *shift_jis* で作られていたため、 ``ablog build`` すると `UnicodeDecodeError` になって進まない・・・

open関数を使っているところを、

.. code-block:: python

   open(path, encode='utf-8')

というふうに書き換えれば一応動きますが、ソースをいじるのも面倒なので、とりあえず Mac で書いてます。

.. note::

   最終的には Mac と Windows 両方で使えるようにしたい。


解決
----

そういえばPython3から、open関数の引数で適切なエンコードを指定しないと開けなくなった、と、何かで見た気がします。
WindowsではPython2.7で動かすようにしたら、上記のエラーは出なくなりました。

.. update:: May 20, 2016

   Python3から encoding 引数が追加され、デフォルトではOSで設定されている既定の文字コードが使われるようです。

   `組み込み関数 - open() <http://docs.python.jp/3.3/library/functions.html#open>`_


テーマ変更がうまくいかない
--------------------------

テーマを変えようとしたら、 ``ablog build`` すると以下のエラーメッセージが出てビルドできませんでした。

.. code-block:: python

   Exception occurred:
     File "c:\python27\lib\site-packages\sphinx\jinja2glue.py", line 159, in get_source
       raise TemplateNotFound(template)
   TemplateNotFound: about.html

やり方を間違えているだけだと思いますが、なんか面倒そうなので、とりあえず保留。

