.. post:: Aug 11, 2017
   :tags: travis, sphinx, ablog
   :category: blog

Server Name Indication チェックが出来ない警告ログ
=================================================

Travisのビルドログをよく見ると、「Server Name Indication (SNI) のチェックが出来ないからリンクが壊れるかも」みたいな警告が出ていた。

ログ内容
--------

ログの内容は以下の通り。
（読みやすいように途中で改行を入れてある）

.. code-block:: python

   /home/travis/virtualenv/python2.7_with_system_site_packages/local/lib/python2.7/site-packages/sphinx/util/requests.py:64: 
   UserWarning: Some links may return broken results due to being unable to check the Server Name Indication (SNI) in the returned SSL cert against the hostname in the url requested. 
   Recommended to install "requests[security]" as a dependency or upgrade to a python version with SNI support (Python 3 and Python 2.7.9+).

で、 ``requests[security]`` を入れるか、Python3 か Python2.7.9以上 を使え、とのこと。


対応
----

*sphinxcontrib.googlechart* を使っている投稿があるのでPython3は使えない（対応していないのでビルドエラーになる）し、 *.travis.yml* でPythonのバージョンを 2.7 にするとTravis上で使われるPythonのバージョンは 2.7.3 になるの（※下記Update参照）で、 ``requests[security]`` を入れる、ということで落ち着いた。

.. update:: Mar 14, 2018

   :ref:`post-apr-07-2018`



*.travis.yml* に、以下の一文を追加。

.. code-block:: diff

      - pip install ablog==0.8.4
      - pip install sphinxcontrib-googlechart
   +  - pip install requests[security]

