.. post:: May 16, 2016
   :tags: ablog, github, sphinx
   :category: blog


Ablogで書いたSphinxブログをGithubにデプロイしてみる
===================================================

*Github* にこのブログをデプロイをしてみる。
ということで、初めてのリポジトリを作成。


いきなりつまづく
----------------

環境は Windows 10。

`Deploy to GitHub Pages <http://ablog.readthedocs.io/manual/deploy-to-github-pages/>`_ の説明通りに ``ablog deploy`` とやると、なにやらエラーが出てデプロイできない・・・


まず
----

そもそも、Githubで2要素認証をオンにしていたことで、そのままではHTTPSで clone や push などができていませんでした。
とりあえずSSH用のキーペアを作って、SSHで push したらデプロイはできました。

（手順は *windows git ssh* 等で検索）


ちなみに
--------

HTTPSでも push できるようにするには、 Github でアクセストークンを生成すればいいらしい。
ということで、アクセストークンを生成して、HTTPSでも push できるようになりました。

（手順は *github access token* 等で検索）


しかし
------

依然として、 ``ablog deploy`` は動きません・・・

::

   git push origin master
   bash: /dev/tty: No such device or address
   error: failed to execute prompt script (exit code 1)
   fatal: could not read Username for 'https://github.com': Invalid argument
   Traceback (most recent call last):
     File "C:\Python27\Scripts\ablog-script.py", line 9, in <module>
       load_entry_point('ablog==0.8.3', 'console_scripts', 'ablog')()
     File "c:\python27\lib\site-packages\ablog\commands.py", line 401, in ablog_main
       namespace.func(**namespace.__dict__)
     File "c:\python27\lib\site-packages\ablog\commands.py", line 389, in ablog_deploy
       run(push, echo=True)
     File "c:\python27\lib\site-packages\invoke\__init__.py", line 27, in run
       return Context().run(command, **kwargs)
     File "c:\python27\lib\site-packages\invoke\context.py", line 53, in run
       return runner_class(context=self).run(command, **kwargs)
     File "c:\python27\lib\site-packages\invoke\runners.py", line 302, in run
       raise Failure(result)
   invoke.exceptions.Failure: Command execution failure!

   Exit code: 128

   Stderr:

   bash: /dev/tty: No such device or address
   error: failed to execute prompt script (exit code 1)
   fatal: could not read Username for 'https://github.com': Invalid argument


Githubのアクセストークンを保存する
----------------------------------

push するときに、毎回コマンドラインにトークンを貼り付けるのは面倒すぎるので、 `Caching your GitHub password in Git <https://help.github.com/articles/caching-your-github-password-in-git/#platform-windows>`_ に書いてある通り、

``git config --global credential.helper wincred``

を実行してから一度 push すると、トークンが Windows資格情報 に保存されます。


デプロイできるようになった
--------------------------

アクセストークンを Windows資格情報 に保存してから、 ``ablog deploy`` したらデプロイできるようになりました！！

めでたしめでたし。


が、しかし
----------

デプロイはできるようになりましたが、このままでは Mac と Windows で横断的にブログを書く、という個人的要件が満たされません。


自動ビルド＆デプロイ
--------------------

ということで、 `Automate GitHub Pages Deploys <http://ablog.readthedocs.io/manual/auto-github-pages-deploys/>`_ を参考に、
Travis CI を使って自動ビルド＆デプロイできるようにしてみました。

これで、WindowsでもMacでも、Githubに push すれば自動的にデプロイまでできるようになり、
さらにブログのソース管理もできて、言うことなしです。

