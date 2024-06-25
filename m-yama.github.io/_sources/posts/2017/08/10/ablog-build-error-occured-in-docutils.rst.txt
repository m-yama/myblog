.. post:: Aug 10, 2017
   :tags: ablog
   :category: blog


ablog build でまたしてもエラー
===============================

Travis でまたしてもビルドエラーになってしまった。
:ref:`前回のエラー<ablog-build-error>` ではSphinxのバージョンを変更して対応したが、
今回は `docutils` 内で発生。

ちなみに、Pythonのバージョンは 2.7 。


エラー内容
----------

.. code-block:: python

   Exception occurred:
     File "/Users/myama/.pyenv/versions/2.7.13/lib/python2.7/site-packages/docutils/nodes.py", line 567, in __getitem__
       return self.attributes[key]
   KeyError: 'source'


ひとまずの解消法
----------------

docutils 0.14 から発生するようになったっぽいので、 `.travis.yml` に以下の一文を追加。

.. code-block:: diff
   
    before_install:
      - pip install sphinx==1.5.6
   +  - pip install docutils==0.13.1
      - pip install ablog

