.. post:: May 22, 2016
   :tags: ablog, sphinx, font
   :category: blog
   :image: 0


Ablogにアイコンが表示されるようにする
=====================================

カレンダーやタグなどのアイコンが表示されていなかったので、表示されるようにする。

.. figure:: images/fontawesome-setting-01.png
   
   ↑こういうアイコン


conf.py を確認
---------------

*conf.py* にはデフォルトで、

.. code-block:: python

   fontawesome_link_cdn = True

と設定されていますが、うまく機能していないようです。マニュアルを見ると、

http://ablog.readthedocs.io/manual/ablog-configuration-options/#fa

::

   fontawesome_link_cdn was a boolean option, and now became a
   string to enable using desired version of Font Awesome.
   To get the old behavior, use ...

とあるので、以下のように変更してみました。

.. code-block:: python

   fontawesome_link_cdn = 'https://maxcdn.bootstrapcdn.com/font-awesome/4.6.3/css/font-awesome.min.css'

アイコンは表示されるようになりましたが、ビルドするときに、

::

   Running Sphinx v1.4.1
   loading translations [ja]... done
   WARNING: The config value `fontawesome_link_cdn' has type `str', defaults to `bool.'

といった警告が出るようになってしまいました。


Font Awesome をリソースに含める
-------------------------------

CDNにリンクするのではなく、サイト内に含めてしまうようにします。

`Font Awesome <http://fontawesome.io/?utm_source=hackernewsletter>`_ からダウンロードし、以下のように配置。

::

   _static/
      css/
         font-awesome.min.css
      fonts/
         *

*conf.py* を以下のように変更。

.. code-block:: python

   #fontawesome_link_cdn = True
   ... 
   fontawesome_css_file = 'css/font-awesome.min.css'

これで、警告もなくアイコンが表示されるようになりました。

