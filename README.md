Synopsis
==========
These are Ruby bindings to Clipper, Angus Johnson's Polygon clipping
library. Because Clipper is not readily packaged, and is so beautifully
self-contained, I've included the two required files in the package.

This release contains version 6.2.1 of Clipper.

* [Clipper Homepage](http://angusj.com/delphi/clipper.php)
* [rbclipper](http://github.com/mieko/rbclipper)

To install:

    gem install clipper

Build locally:

    rake install


Simple Usage:
===========
This shold be enough to get you started.  Full documentation is below.

    require 'clipper'

    a = [[0, 0], [0, 100], [100, 100], [100, 0]]
    b = [[-5, 50], [200, 50], [100, 5]]

    c = Clipper::Clipper.new

    c.add_subject_polygon(a)
    c.add_clip_polygon(b)
    c.union :non_zero, :non_zero

    => [[[100.0, 0.0], [0.0, 0.0], [0.0, 47.85714326530613], [-4.999999, 50.0],
         [0.0, 50.0], [0.0, 100.0], [100.0, 100.0], [100.0, 50.0],
         [200.0, 50.0], [100.0, 5.0]]]

Documentation
================

Clipper is a two-dimensional polygon clipping library.  `rbclipper`, the Ruby
bindings can be accessed by:

    require 'clipper'


Polygons
--------
Operations that accept or return polygons are specified as an array of `[x,y]`
coordinates, for example, to specify a triangle:

    triangle = [[0,0], [0,100], [50, -100]]

Clipper supports both holes and complex polygons.  Coordinates for output
polygons are clockwise for shells, and and counter-clockwise for holes.
See force_orientation.

Note that since 2.8, Clipper defines orientation with respect to a
_downward-increasing Y axis_, similar to how many 2D GUI/drawing APIs position
coordinate (0,0) at the top-left corner.  The bindings have followed Clipper
proper in this regard.

Multiple polygons are represented as simply an array of polygons.

Fill Types
-----------
  * `:even_odd`

    A point is considered inside the polygon if the number of edge-crossings to
    get there from outside the shape is an even number.

  * `:non_zero`

    A point is considered inside the polygon if the number of edge-crossings to
    get there is greater than zero.

Clipper::Clipper Methods
-------

* `Clipper#initialize`

   Creates a new clipper object.

* `Clipper#add_subject_polygon(polygon)`

  `Clipper#add_clip_polygon(polygon)`

  Adds a subject or clip polygon to the engine.  Boolean operations are
  calculated as `SUBJECT` *operatation* `CLIP`.  Multiple polygons can Pay attention
  to the orientation of the coordinates given; counter-clockwise for shells and
  clockwise for holes.

  Multiple subject and clip polygons can be added to the engine for operations.

* `Clipper#add_subject_poly_polygon(poly_polygon)`

  `Clipper#add_clip_poly_polygon(poly_polygon)`

  Add a "Poly-Polygon" to the engine.  Which is basically a set of polygons.  
  Boolean operations consider every poly-polygon added in this manner to be the
  same object.

* `Clipper#force_orientation`

  `Clipper#force_orientation=`

  Defaults to true.  Ensures that the simple result of boolean operations have
  the orientation as described in section Polygons.  Only useful with simple
  polygons.

* `Clipper#intersection(subject_fill=:even_odd, clip_fill=:even_odd)`

  `Clipper#union(subject_fill=:even_odd, clip_fill=:even_odd)`

  `Clipper#difference(subject_fill=:even_odd, clip_fill=:even_odd)`

  `Clipper#xor(subject_fill=:even_odd, clip_fill=:even_odd)`

   Performs a boolean operation on the polygons that have been added to the
   clipper object.  The result is a list of polygons.
