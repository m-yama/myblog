.. post:: Jul 17, 2017
   :tags: ablog
   :category: blog


ablog build でエラー
=====================

``ablog build`` を実行すると、ローカルではビルドが通るが、GithubにプッシュしてTravisでビルドが実行された時に、

.. code-block:: python

   ImportError: cannot import name make_admonition in Sphinx 1.6

というエラーが発生するようになった。


ひとまずの解消法
-----------------

Shinx 1.6 から発生するようになったエラーのようなので、 `.travis.yml` に以下の一文を追加。

.. code-block:: yaml

    before_install:
   +  - pip install sphinx==1.5.6
      - pip install ablog
