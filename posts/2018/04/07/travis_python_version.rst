.. post:: Apr 07, 2018
   :tags: travis
   :category: blog

.. _post-apr-07-2018:

Travis-CIでのPythonバージョンの変更
===================================

このブログは `Ablog <http://ablog.readthedocs.io/>`_ を使用しており、ブログを書いてgithubにpushした時に、自動的にビルドしてgithub.ioにデプロイするためにTravis-CIを使用している。

Travis-CIでPython3.6を使おうと .travis.yaml のPythonバージョンを書き換えたところ、
エラーが発生してビルドできない状態になった。

エラーログは以下の通り。

.. code-block:: sh

   $ source ~/virtualenv/python3.6_with_system_site_packages/bin/activate
   /home/travis/.travis/job_stages: line 57: /home/travis/virtualenv/python3.6_with_system_site_packages/bin/activate: No such file or directory
   The command "source ~/virtualenv/python3.6_with_system_site_packages/bin/activate" failed and exited with 1 during .
   Your build has been stopped.


調べてみると、Travis-CIのサイトで以下のように説明があった。

https://docs.travis-ci.com/user/languages/python/

----

   The CI Environment uses separate virtualenv instances for each Python version.
   This means that as soon as you specify language: python in .travis.yml your tests will run inside a virtualenv (without you having to explicitly create it).
   System Python is not used and should not be relied on. 
   If you need to install Python packages, do it via pip and not apt.

   （Travis CIの環境では、Pythonのバージョンごとに個別のvirtualenvインスタンスを使用します。
   つまり、 language: python と記述することで、あなたのテストはvirtualenv内で動作するようになります。（明示的に作成する必要はありません）
   システムのPythonは使用されませんし、依存してはいけません。
   もしPythonパッケージをインストールする必要があるなら、aptではなくpip経由でインストールしてください。）

   If you decide to use apt anyway, note that for compatibility reasons, you’ll only be able to use the default Python versions that are available in Ubuntu (e.g. for Trusty, this means 2.7.6 and 3.4.3). 

   （とにかくaptを使うと決めた場合は、互換性の理由により、Ubuntuで使用可能なデフォルトのPythonバージョンしか使用できないことに注意して下さい。（例えば Trustyでは、2.7.6 と 3.4.3 ということです））

   To access the packages inside the virtualenv, you will need to specify that it should be created with the --system-site-packages option. To do this, include the following in your .travis.yml:

   （virtualenv内のパッケージにアクセスするには、 ``--system-site-packages`` オプション付きで作成されるよう指定する必要があります。そうするには、 .travis.yml に以下の内容を含めてください。）

   .. code-block:: yaml

      language: python
      virtualenv:
        system_site_packages: true

----

要は、 ``system_site_packages`` が *true* になっていると、システムのデフォルトのPythonを使用する、ということらしい。

今回のエラーは、Ablogを使い始めた時に何も調べずAblogのサイトにあったサンプルの .travis.yaml をそのまま使っていたため、 ``system_site_packages`` が *true* になっていたことが原因だった。

エラーを解消するには、 ``system_site_packages`` を *false* にするか、必要なければ virtualenv のセクション自体削除することで、
使いたいバージョンのPythonを Travis-CI 上で使えるようになる。

