.. post:: 2019-08-19
   :tags: powershell
   :category: blog

Powershellで改行コードをCRLFに一括置換する
==========================================

Powershellで、フォルダ内（サブフォルダ含む）のファイルの改行コードをCRLFに一括置換する。


CSVだけ対象にし、文字コードがShift_JISの場合
--------------------------------------------

.. code-block:: powershell

   ls -r -file -filter *.csv | % { (get-content -encoding Default $_.FullName) -join "`r`n" | set-content -encoding Default $_.FullName }


CSV以外を対象にし、文字コードがUTF8の場合
-----------------------------------------

.. code-block:: powershell

   ls -r -file -exclude *.csv | % { (get-content -encoding UTF8 $_.FullName) -join "`r`n" | set-content -encoding UTF8 $_.FullName }


* ``-filter`` で対象のファイルの拡張子を指定。
* ``-exclude`` で除外するファイルの拡張子を指定。
* ```r`n`` を ```n`` に書き換えれば、LFに一括置換になるが、最後にCRLFの改行が入る。最後の改行を省くには ``set-content -NoNewline`` とする。

.. note::

   * 上記スクリプトはファイル自体が書き換わるので注意。
   * UTF8はBOM付きになるので注意。BOM無しにするには ``set-content`` ではなく .NET Framework の ``[System.IO.File]::WriteAllLines`` メソッドなどを使う。

