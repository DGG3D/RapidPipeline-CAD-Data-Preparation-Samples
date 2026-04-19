# no.468 gt4rs.stp

## Summary

Porsche 718 Cayman GT4 RS

## Link

https://grabcad.com/library/porsche-718-cayman-gt4-rs-1

## Screenshots

![screenshot](<screenshot/no.468 gt4rs.jpg>)  

![screenshot](<screenshot/no.468 gt4rs_explode.jpg>)  

## Description

Work in Alias2021,rendered in Corona8.
exterior mirror, windshield wiper, brakes,front logo and interior are made by kunos(Assetto Corsa).  

## Purpose

This sample CAD asset demonstrates quite a few challenges for a propper conversion from parametric CAD to polygonal mesh:

### Surface Patches
![screenshot](<screenshot/no.468 gt4rs_patches.jpg>)  
![screenshot](<screenshot/no.468 gt4rs_patches-viz.jpg>)  
1. The parametric surface was created through patches which are non-solids and allow for the creation of non-planar, complex 3D surfaces. However these patches present quite a large challenge for the tessellation and mesh processing step. Ideally they are already merged  on the b-rep level before any tessellation.  

### Winding Order Issues
![screenshot](<screenshot/no.468 gt4rs_windingOrder.jpg>)  
2. Winding Order Issues. These usually stem from translation issues from parametric CAD to mesh surface, but can also have their origin within the CAD data set for example due to intersecting surfaces or open edges.  


## Author

aria - https://grabcad.com/aria-16 

## Legal

[GrabCad Terms](https://grabcad.com/terms)
[GrabCad IP Policy](https://grabcad.com/ip_policy)