================================================================================
Stacked ones
================================================================================


--------------------------------------------------------------------------------
Rational
--------------------------------------------------------------------------------

Why author start this
----------------------------------------
I tried to use two extensions for this editor.
Syntax higilight had gone ruin on both extensions.
The problem was miss-highlight for link.
Single backquote (`) within code-block was handled as 'beginning of link',
so from that point to next appearance of 'end of link', 
highlight was stretching all the way.

I neither wrote complex reST documents nor needed aggressive highlight.
So if possible, fixing existing one was desirable.
But it was not possible.
Because they did not handle 'block' and 
needed to change 'link' from multiline-capable to singleline-limited to fix the problem, 
it must to have broken very many user's workspace.

I gave up that plan, and shifted to this project.


--------------------------------------------------------------------------------
Facts
--------------------------------------------------------------------------------

Very hard, irritating, sometimes no gain for some works, and anyway not fun at all.
regexp by myself is difficult to keep simple and clear and maintenancable,
and user-inputs, including ones done by me, are very bad.
I do not, and did not want to elaborate such a thing again.
So logging was necessary.


Begin with inspection of the block with indentation
--------------------------------------------------------------------------------

* 'begin' part is first indent (only space, exclude tab).
* 'end' part is blank line, or line contains only spaces.
* Remains are correspond to 'contentName' part, starts from the head of non-space part of first line.


.. code-block:: rst

  This part is not block.

    A this should be 1st line of block
    afkdlA testtesttest `some <https://example.com/short>`_
    one more `a <url>`_ bar
    we exclude the case of `multiline link
    <https://example.com/multiline>`_
    1stBlocksLastLine

  This part is not block again.


.. code-block:: json

   {
     "$schema": "https://raw.githubusercontent.com/martinring/tmlanguage/master/tmlanguage.json",
     "name": "reST",
     "patterns": [
       {
         "include": "#block"
       }
     ],
     "repository": {
       "block": {
         "patterns": [
           {
             "name": "markup.italic",
             "begin": "^[ ]+(?=[^ ].+$)",
             "end": "^[ ]*$",
             "contentName": "markup.bold"
           }
         ]
       }
     },
     "scopeName": "text.reST"
   }


We can use local "repository" key
--------------------------------------------------------------------------------

In the following simplified code block, ``fieldsome`` inside 'dirImage' can be recognized.


.. code-block:: rst

   .. image:: some0.jpg
   
   .. image:: some1.jpg
      :height: 100px
      :width: 120px
      :unknown: some
      :align: right


.. code-block:: json

   {
     "$schema": "https://raw.githubusercontent.com/martinring/tmlanguage/master/tmlanguage.json",
     "name": "reST",
     "patterns": [
       {
         "include": "#dirImage"
       }
     ],
     "repository": {
       "dirImage": {
         "patterns": [
           {
             "name": "markup.italic",
             "begin": "^\\.\\. image:: .+",
             "end": "^$",
             "patterns": [
               {
                 "include": "#fieldsome"
               }
             ]
           }
         ],
         "repository": {
           "fieldsome": {
             "patterns": [
               {
                 "name": "constant.regexp",
                 "match": "^ +:\\w+: [\\s\\S]+$"
               }
             ]
           }
         }
       }
     },
     "scopeName": "text.reST"
   }


We also use parent 'repository', like a code below.


.. code-block:: json

   {
     "$schema": "https://raw.githubusercontent.com/martinring/tmlanguage/master/tmlanguage.json",
     "name": "reST",
     "patterns": [
       {
         "include": "#dirImage"
       }
     ],
     "repository": {
       "dirImage": {
         "patterns": [
           {
             "name": "markup.italic",
             "begin": "^\\.\\. image:: .+",
             "end": "^$",
             "patterns": [
               {
                 "include": "#fieldsome"
               }
             ]
           }
         ]
       },
       "fieldsome": {
         "patterns": [
           {
             "name": "constant.regexp",
             "match": "^ +:\\w+: [\\s\\S]+$"
           }
         ]
       }
     },
     "scopeName": "text.reST"
   }


At first, 'Heading' had two type as block
--------------------------------------------------------------------------------

At first, 'Heading' had two type as block.

* One begins with marker line. (if exists, detected at first, as heading block)
* Another begins with heading-line string. (just as normal non-indented block, not as heading block)
* Both can not be indented. (this may be not good)

And it was changed after non-indented block implemented,
because the type does not begin with heading-line string becomes just block 
which contains heading-line as second line.
To Separate with 'just block' and assign heading-block specific field list, is impossible.
So heading-blocks are all just block contain heading-line string by chance.


Not exhaust directive specific args nor options
--------------------------------------------------------------------------------

We can still distinguish each reST directives, and assign each one's specific field list.
But it involve many works and I believe there does not exist very much demand, 
so I passed on exhaust them. 
`knot: remove support for image directive and fieldlist · PrsPrsBK/vs-reST-highlight@d08b25d <https://github.com/PrsPrsBK/vs-reST-highlight/commit/d08b25d5df43788ac15af4f666f9c2245c3f36bb>`_


Backquote
--------------------------------------------------------------------------------

Expression for 'neither prefixed with ``\`` nor ````` is ``(?<![`\\\\])`[^_`]`` (backslash 4 times).
This allow us to handle escaped backquote in reST document.


We can not handle both a link written in multiline and alone backquote
--------------------------------------------------------------------------------

I made a issue (2018-09-13 JST):
`Syntax rule 'begin-end' stretch over too much, and extend it's parent range out · Issue #58551 · Microsoft/vscode <https://github.com/Microsoft/vscode/issues/58551>`_



