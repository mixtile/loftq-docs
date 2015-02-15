Sample
========
:Author: author <author@author.com>
:Authors: - author1 <author1@author.com>
          - author2 <author2@author.com>
:Organization: organization
:Contact: contact information
:Address: address information
:Version: version 0.1
:Status: status Info
:Date:  2015-01-27
:Copyright: copyright Info
:Dedication: topic.
:Abstract: This is the sample article for indroducing how to use RestructuredText.



list example
-------------

bulleted list
''''''''''''''

* This is a bulleted list.
* It has two items, the second
  item uses two lines. (note the indentation)

numberd list
''''''''''''''

1. This is a numbered list.
2. It has two items too.

#. This is a numbered list.
#. It has two items too.


table example
---------------


first sample
'''''''''''''''

+---------+---------+-----------+
| 1       |  2      |  3        |
+---------+---------+-----------+

second sample
'''''''''''''''

+---------------------+---------+---+
|1                    |        2| 3 |
+---------------------+---------+---+


multle table first sample
''''''''''''''''''''''''''''''

+------------+------------+-----------+
| Header 1   | Header 2   | Header 3  |
+============+============+===========+
| body row 1 | column 2   | column 3  |
+------------+------------+-----------+
| body row 2 | Cells may span columns.|
+------------+------------+-----------+
| body row 3 | Cells may  | - Cells   |
+------------+ span rows. | - contain |
| body row 4 |            | - blocks. |
+------------+------------+-----------+


multle table second sample
''''''''''''''''''''''''''''''

=====  =====  ======
   Inputs     Output
------------  ------
  A      B    A or B
=====  =====  ======
False  False  False
True   False  True
=====  =====  ======



The tabularcolumns directive
'''''''''''''''''''''''''''''

.. tabularcolumns:: |l|c|p{5cm}|

+--------------+---+-----------+
|  simple text | 2 | 3         |
+--------------+---+-----------+



The csv-table directive
'''''''''''''''''''''''''

.. csv-table:: a title
   :header: "name", "firstname", "age"
   :widths: 20, 20, 10

   "Smith", "John", 40
   "Smith", "John, Junior", 20


images and figures samples
------------------------------


include a image
'''''''''''''''''''''''

.. image:: _static/pictures/mixtile-logo.png
    :width: 200px
    :align: center
    :height: 100px
    :alt: alternate text


include a figure
'''''''''''''''''''''

.. figure:: _static/pictures/loftq-view.png
    :width: 480px
    :align: center
    :height: 320px
    :alt: alternate text

    figure are like images but with a caption and whatever else youwish to add



Boxes 
------------------


Boxes include Note, Warn, SeeAlso, etc.

Colored boxes: note, seealso, todo and warnings
'''''''''''''''''''''''''''''''''''''''''''''''''


See Also Box as Below:

.. seealso:: This is a simple **seealso** note.


Note Box as Below:

.. note::  This is a **note** box.


Warning Box as Below:

.. warning:: note the space between the directive and the text


Topic directive
''''''''''''''''''''

.. topic:: Your Topic Title

    Subsequent indented lines comprise
    the body of the topic, and are
    interpreted as body elements.


Sidebar directive
''''''''''''''''''''

.. sidebar:: Sidebar Title
    :subtitle: Optional Sidebar Subtitle

    Subsequent indented lines comprise
    the body of the sidebar, and are
    interpreted as body elements.


Misc
-------------


comments
'''''''''''''''


.. these text will not show up in sample article.



Substitutions
'''''''''''''''''''

.. _Python: http://www.python.org/


.. |longtext| replace:: this is a very very long text to include



glossary, centered, index, download and field list
''''''''''''''''''''''''''''''''''''''''''''''''''''''

fileld list example:

:Whatever: this is handy to create new field


glossary example:

  .. glossary::
     apical
        at the top of the plant.


index example:


  .. index::
    1. first index
    2. second indexx 

download example:

  :download:`download samplet.py <sample.py>`


hlist directive example:

.. hlist::
    :columns: 3

    * first item
    * second item
    * 3d item
    * 4th item
    * 5th item
















