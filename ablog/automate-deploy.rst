.. post:: May 18, 2016
   :tags: ablog


自動ビルド＆デプロイ
====================

:ref:`前回の投稿<deploy-to-github>` でデプロイはできるようになったが、
このままでは Mac と Windows で横断的にブログを書く、という要件が満たされない。

ということで、 `Automate GitHub Pages Deploys <http://ablog.readthedocs.io/manual/auto-github-pages-deploys/>`_ を参考に、 Travis CI を使って自動ビルド＆デプロイできるようにする。


