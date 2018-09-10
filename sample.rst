========================================
Sample of reST
========================================

dfkjfkdajiujadgaerhgegl

  A this should be 1st line of block
  afkdlA testtesttest `some <https://example.com/short>`_
  one more `a <url>`_ bar
  we exclude the case of `multiline link
  <https://example.com/multiline>`_
  1stBlocksLastLine

ch1
--------------

.. image:: some0.jpg

.. image:: some1.jpg
   :height: 100px
   :width: 120px
   :unknown: some
   :align: right

oh image directive.

* `link in listitem <https://example.com/link01/>`__ EEEEE.
* インストールする ``C:\Program Files\sample\`` 選べる。
  2018-03-21 continuing line ``D:\bin\example\1.0.0\sample.exe``
  
  - 2018-09-01 ``not-link.exe`` そのまま。
    継続する行 ``E:/bin/example/preview/`` になった
  - 次のアイテム
  - エスケープしたバッククォート \` <->
    continuing line >\`_ not link

* \` start from escaped backquote <-->
* `link item include \`escaped/backquote\` what's happen \`..\` Let's go <https://example.com/issues/1234>`__

  - ``/../..`` start from backquote

* `normal link <https://example.com/link02>`_

  - just literal url https://github.com/obstschale/octave-docset. 
    fin.


Ch2
--------------

* single item


.. code-block:: console

  # comment
  # next
  mkdir (1..99 | % { "Dir{0:00}" -f $_}) # tail comment

  # second block
  @(, $array)
  $foo = ,$array

  # third block
  foreach($line in (Get-Content $file)) {
    if($line -match "^(\d+?):.*") {
      $o [$matches[1]]
    }
  }

  <#
   # fourth block
   # multiline comment
   # powershell uses backquote when escape charactor
  #>
  "Hello `n World" -match '(?s)Hello.*World'  # code-block, so we do not mean beginning of link
  "foo".Replace('x', 'y')
  "foo bar" -replace '(.*) (.*)', '$2 $1'
  # powershell escape example `n means newline
  "Hello" -eq "HELLO" # True
  "Hello World".TrimEnd("World")

If we failed, this zone is interpreted as a part of link.
`link stopped? <https://example.com/ohno>`_
Are we recovered here?

