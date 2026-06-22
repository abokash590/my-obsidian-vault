Computational Geometry
What does that
mean?
Computational Geometry
Some basic algorithms
Given directed line segments p0p1 and  p0p2,
determine whether  p0p1 is clockwise from  p0p2 with
respect to point  p0?
Given two line segments p0p1 and  p1p2, if we
traverse  p0p1 and then  p1p2, do we make a left
turn at point  p1?
Do line segments  p0p1 and  p2p3 intersect?
Computational Geometry
Cross Products
p2 = (x2,y2)
y
(0,0)
p1 = (x1,y1)
p1  p2 = x1y2 – x2y1
We assume that cross product is scalar given by this formula...
= – p2  p1
x
Computational Geometry
Cross Products
y
(0,0)
p2 = (x2,y2)
p1 + p2
p1 = (x1,y1)
x
p1  p2 = x1y2 – x2y1
= – p2  p1
Computational Geometry
Cross Products
y

p1
–
(0,0)
x
The sign (+ or –) of cross
product depends in which
half plane (relative to p1)
lies p2
Computational Geometry
Some basic algorithms
Given directed line segments p0p1 and  p0p2,
determine whether  p0p1 is clockwise from  p0p2 with
respect to point  p0?
Compute  =  p1  p2
if  > 0, then p0p1 is clockwise from  p0p2
if  < 0, then p0p1 is counterclockwise from  p0p2
or,
if  > 0, then p0p2 is counterclockwise from  p0p1
if  < 0, then p0p2 is clockwise from  p0p1
Computational Geometry
Some basic algorithms
Given two line segments p0p1 and  p1p2, if we
traverse  p0p1 and then  p1p2, do we make a left
turn at point  p1?
p2
Compute  =  (p2 – p0)  (p1 – p0)
if  > 0, then we make a right turn at p1
if  < 0, then we make a left turn at p1
p1
p0
Computational Geometry
Two Segments Intersect?
One method:  solve for the intersection point of the two lines containing the
two line segments, and then check whether this point lies on both segments.
In practice, the two input segments often do not intersect.
Stage 1:  quick rejection if their bounding boxes do not intersect
if and only if
x <  x  x > x  y < y  y > y
4          1             3        2             4        1              3        2
L right of M?
L left of M?      L above M?      L below M?
Case 1: bounding boxes
do not intersect; neither
will the segments.
(x  ,  y )
4      4
M
(x  , y )
3     3
bounding boxes
(x  , y )
2     2
L
(x  , y )
1     1
Computational Geometry
Bounding Box
Case 2: Bounding boxes intersect; the segments may or may
not intersect.  Needs to be further checked in Stage 2.
Computational Geometry
Bounding Box - Stage 2
Two line segments do not intersect if and only if one segment
lies entirely to one side of the line containing the other segment.
p1
p3
p2
p4
3         2           1       2
( p    p  )  ( p   p )  and
( p  p  )  ( p  p )  are
both positive!
4         2           1       2
Computational Geometry
Necessary and Sufficient Condition
Two line segments intersect iff each of the two pairs of cross
products below have different signs (or one cross product in
the pair is 0).
(p   p )  ( p  p ) and  ( p   p  )  ( p  p )
1         4            3       4                  2        4            3        4
(p  p )  ( p  p  ) and  ( p   p )  ( p   p )
2           1        2                  4        2           1        2
3
p
1
p
3
p3
p1
3
// the line through p  p
4
// intersects  p p .
2
// the line through p  p
2
// intersects p p . 4
1
1
3
p
4
p
2
p4
p2
Computational Geometry
Line Segment Intersection Algorithm
• See page: 937
Computational Geometry
• Data is defined as points, lines, surfaces, polygons etc.
Convex                              Non-convex
 Convex Polygon has every in degree less than 180
 Non Convex Polygon may have one or more in degrees
greater than 180
Computational Geometry
Convex Set
• an object is convex if for every pair of
points within the object, every point on the
straight line segment that joins them is also
within the object.
Computational Geometry
• Problem: To determine whether a given point lies outside
or inside a given polygon.
• Answer: Yes if the point lies inside the polygon, No
otherwise
• Solution: Draw a line along the X –axis from the point in
one direction i.e. the line can have a increasing as well as
decreasing X – axis.
If the line thus drawn intersects ONLY once with any edge
of the polygon then the point lies inside the polygon else it
lies outside the polygon
•
Computational Geometry
• Example:
_ _ _ _ _ _ _ _ _ __ _ _.P
The above point intersects only once with an edge of the polygon
hence it lies inside polygon
Computational Geometry
Convex Hull - Problem
Given n points on plane  p1, p2,  ,pn, find the smallest
convex polygon that contains all points p1, p2,  ,pn.
Computational Geometry
Convex Hull - Example
Given n line points on plane  p1, p2,  ,pn, find the smallest
convex polygon that contains all points p1, p2,  ,pn.
p12
p10
p6
p11
p9
p0
p7
p8
p5
p4
p2
p3
p1
Computational Geometry
Convex Hull - Example
Given n line points on plane  p1, p2,  ,pn, find the smallest
convex polygon that contains all points p1, p2,  ,pn.
p12
p10
p6
p11
p9
p0
p7
p8
p5
p4
p2
p3
p1
Computational Geometry
Graham Scan - Algorithm
procedure GrahamScan(set Q)
let p0 be the point with the minimum y-coordinate
let <p1, , pm> be the remaining points in  Q, sorted by the
angle in counterclockwise order around p0
Top(S)  0
Push(p0, S); Push(p1, S); Push(p2, S)
for i  3 to m do
while the angle formed by points NextToTop(S), Top(S)
and pi makes a non-left turn do
Pop(S)
Push(pi, S)
return S
Computational Geometry
Graham Scan - Example
p10
p9
p6
p7
p12
p11
p0
p8
p5
p4
p2
p3
p1
Computational Geometry
Graham Scan - Example
p10
p9
p6
p7
p12
p11
p0
p8
p5
p4
p2
p3
p1
Computational Geometry
Graham Scan - Example
p10
p9
p6
p7
p12
p11
p0
p8
p5
p4
p2
p3
p1
Computational Geometry
Graham Scan - Example
p10
p9
p6
p7
p12
p11
p0
p8
p5
p4
p2
p3
p1
Computational Geometry
Graham Scan - Example
p10
p9
p6
p7
p12
p11
p0
p8
p5
p4
p2
p3
p1
Computational Geometry
Graham Scan - Example
p10
p9
p6
p7
p12
p11
p0
p8
p5
p4
p2
p3
p1
Computational Geometry
Graham Scan - Example
p10
p9
p6
p7
p12
p11
p0
p8
p5
p4
p2
p3
p1
Computational Geometry
Graham Scan - Example
p10
p9
p6
p7
p12
p11
p0
p8
p5
p4
p2
p3
p1
Computational Geometry
Graham Scan - Example
p10
p9
p6
p7
p12
p11
p0
p8
p5
p4
p2
p3
p1
Computational Geometry
Graham Scan - Example
p10
p9
p6
p7
p12
p11
p0
p8
p5
p4
p2
p3
p1
Computational Geometry
Graham Scan - Example
p10
p9
p6
p7
p12
p11
p0
p8
p5
p4
p2
p3
p1
Computational Geometry
Graham Scan - Example
p10
p9
p6
p7
p12
p11
p0
p8
p5
p4
p2
p3
p1
Computational Geometry
Graham Scan - Example
p10
p9
p6
p7
p12
p11
p0
p8
p5
p4
p2
p3
p1
Computational Geometry
Graham Scan - Algorithm
procedure GrahamScan(set Q)
let p0 be the point with the minimum y-coordinate
let <p1, , pm> be the remaining points in  Q, sorted by the
angle in counterclockwise order around p0
Top(S)  0
Push(p0, S); Push(p1, S); Push(p2, S)
for i  3 to m do
while the angle formed by points NextToTop(S), Top(S)
and pi makes a non-left turn do
Pop(S)
Push(pi, S)
return S
Computational Geometry
Graham Scan - Complexity
Sorting takes (n log n) time
for loop executes (n) times
each while loop might take up (n) time
however, no more than (n) for all while loops together
procedure GrahamScan(set Q)
let p0 be the point with the minimum y-coordinate
let <p1, , pm> be the remaining points in  Q, sorted by the
angle in counterclockwise order around p0
Top(S)  0
Push(p0, S); Push(p1, S); Push(p2, S)
for i  3 to m do
while the angle formed by points NextToTop(S), Top(S)
and pi makes a non-left turn do
Pop(S)
Push(pi, S)
return S
T(n) = (n log n) + const (n) = (n log n)