.. post:: 2020-11-12
   :tags: ablog
   :category: blog

Ablogのバージョンをアップグレード
=================================

今まで ablog バージョンを指定して古いバージョンを使い続けていましたが、気が向いたので現行の最新バージョン（0.10.12）に更新しました。


ablog deploy できなくなった
---------------------------

記事を書いて github に push すると、Travis でビルドされて github.io に自動でデプロイされる、という仕組みで運用しているのですが、 ``ablog deploy`` のところでコケるようになってしまいました。

github と Travis は https で連携するようにしていたのですが、どうやら ssh で clone しようとしてエラーになっているようです。

.. code-block:: sh

   ablog deploy --push-quietly --github-token=DEPLOY_KEY -m="`git log -1 --pretty=%B`"
   Cloning into '/home/travis/build/m-yama/myblog/m-yama.github.io'...
   
   Permission denied (publickey).
   fatal: Could not read from remote repository.
   
   Please make sure you have the correct access rights
   and the repository exists.
   git clone git@github.com:m-yama/m-yama.github.io.git /home/travis/build/m-yama/myblog/m-yama.github.io
   Traceback (most recent call last):
     File "/home/travis/virtualenv/python3.7.1/bin/ablog", line 8, in <module>
       sys.exit(ablog_main())
     File "/home/travis/virtualenv/python3.7.1/lib/python3.7/site-packages/ablog/commands.py", line 510, in ablog_main
       namespace.func(**namespace.__dict__)
     File "/home/travis/virtualenv/python3.7.1/lib/python3.7/site-packages/ablog/commands.py", line 447, in ablog_deploy
       echo=True,
     File "/home/travis/virtualenv/python3.7.1/lib/python3.7/site-packages/invoke/__init__.py", line 48, in run
       return Context().run(command, **kwargs)
     File "/home/travis/virtualenv/python3.7.1/lib/python3.7/site-packages/invoke/context.py", line 94, in run
       return self._run(runner, command, **kwargs)
     File "/home/travis/virtualenv/python3.7.1/lib/python3.7/site-packages/invoke/context.py", line 101, in _run
       return runner.run(command, **kwargs)
     File "/home/travis/virtualenv/python3.7.1/lib/python3.7/site-packages/invoke/runners.py", line 363, in run
       return self._run_body(command, **kwargs)
     File "/home/travis/virtualenv/python3.7.1/lib/python3.7/site-packages/invoke/runners.py", line 422, in _run_body
       return self.make_promise() if self._asynchronous else self._finish()
     File "/home/travis/virtualenv/python3.7.1/lib/python3.7/site-packages/invoke/runners.py", line 489, in _finish
       raise UnexpectedExit(result)
   invoke.exceptions.UnexpectedExit: Encountered a bad command exit code!
   
   Command: 'git clone git@github.com:m-yama/m-yama.github.io.git /home/travis/build/m-yama/myblog/m-yama.github.io'
   
   Exit code: 128
   
   Stdout: already printed
   
   Stderr: already printed
   
   Done. Your build exited with 0.


解決法（？）
------------

結論から言うと、 ``ablog deploy`` コマンドに ``--github-ssh`` オプションを付けると動くようになります。

*.travis.yml*

.. code-block:: yaml

   after_success:
      ...
      - ablog deploy --push-quietly --github-token=DEPLOY_KEY -m="`git log -1 --pretty=%B`" --github-ssh

しかし、 ``--github-ssh`` オプションを付けると http を使うようになる、というのもおかしい気がします。ネーミングのミスでしょうか？

とりあえず動くようにはなりました。

.. code-block:: sh

   ablog deploy --push-quietly --github-token=DEPLOY_KEY -m="`git log -1 --pretty=%B`" --github-ssh
   Cloning into '/home/travis/build/m-yama/myblog/m-yama.github.io'...
   git clone https://github.com/m-yama/m-yama.github.io.git /home/travis/build/m-yama/myblog/m-yama.github.io
   Moved 159 files to m-yama.github.io
   ...
