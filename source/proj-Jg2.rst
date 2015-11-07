-Jg：Perspective projection
===========================

The perspective projection imitates in 2 dimensions the 3-dimensional
view of the earth from space. The implementation in GMT is very
flexible, and thus requires many input variables. Those are listed and
explained below, with the values used in
Figure :ref:`Perspective projection <GMT_perspective>` between brackets.

-  Longitude and latitude of the projection center (4ºE/52ºN).

-  Altitude of the viewer above sea level in kilometers (230 km). If
   this value is less than 10, it is assumed to be the distance of the
   viewer from the center of the earth in earth radii. If an "r" is
   appended, it is the distance from the center of the earth in
   kilometers.

-  Azimuth in degrees (90, due east). This is the direction in which you
   are looking, measured clockwise from north.

-  Tilt in degrees (60). This is the viewing angle relative to zenith.
   So a tilt of 0º is looking straight down, 60º is looking from 30º above
   the horizon.

-  Twist in degrees (180). This is the boresight rotation (clockwise) of
   the image. The twist of 180º in the example mimics the fact that the
   Space Shuttle flies upside down.

-  Width and height of the viewpoint in degrees (60). This number
   depends on whether you are looking with the naked eye (in which case
   you view is about 60º wide), or with binoculars, for example.

-  Scale as 1:xxxxx or as radius/latitude where radius is distance on
   map in inches from projection center to a particular
   oblique latitude (**-Jg**), or map width (**-JG**) (5 inches).

The imagined view of northwest Europe from a Space Shuttle at 230 km
looking due east is thus accomplished by the following
:doc:`pscoast` command:

   ::

    gmt pscoast -Rg -JG4/52/230/90/60/180/60/60/5i -Bx2g2 -By1g1 -Ia -Di -Glightbrown \
                -Wthinnest -P -Slightblue --MAP_ANNOT_MIN_SPACING=0.25i > GMT_perspective.ps

.. _GMT_perspective:

.. figure:: /images/GMT_perspective.*
   :width: 500 px
   :align: center

   View from the Space Shuttle in Perspective projection.
