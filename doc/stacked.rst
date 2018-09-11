================================================================================
Stacked ones
================================================================================


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


We cannot use local "repository" key
--------------------------------------------------------------------------------


In the following code block, ``fieldsome`` inside 'dirImage' had never been recognized.
I confirmed that the duplication of name 'fieldsome' did not exist.


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
       },
     },
     "scopeName": "text.reST"
   }


We must adhere parent 'repository', like a code below.
The bad thing was, this behavior did not consist with textmate's document.


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
     }
     "scopeName": "text.reST"
   }


