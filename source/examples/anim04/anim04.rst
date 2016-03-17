.. _anim04:

Flying over topography
-----------------------

Our next animation simulates what an imaginary satellite might see as it
passes in a great circle from New York to Miami at an altitude of 160
km. We use the general perspective view projection with
:doc:`grdimage </grdimage>` and use
:doc:`project </project>` to create a great circle path
between the two cities, sampled every 5 km. The main part of the script
will make the DVD-quality frames from different view points, draw the
path on the ground, and add frame numbers to each frame. As this
animation generates 355 frames we can use 3rd party tools to turn the
image sequence into a MPEG-4 movie. Note: At the moment,
:doc:`grdview </grdview>` cannot use general perspective
view projection to allow "fly-through" animations like Fledermaus; we
expect to add this functionality in a future version.

.. literalinclude:: ./anim04.sh
   :language: bash

.. only:: html

   .. figure:: ./anim04.*
      :width: 400px
      :align: center

.. only:: latex

   由于PDF文件不支持显示动图，具体效果见\ `网页版 <http://gmt5.seisman.info/examples/anim04/anim04.html>`_ 。
