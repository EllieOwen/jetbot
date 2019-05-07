Jud5 is a C++ package Including: Machine Vision Library, Statistical Physics Simulator, Appearance Modeling Library and GUI.

The geometric part of the Machine Vision Library is on version 25 and my best work. It is two collections: a bunch transformations, intersections, merges that do PCA (Principle Component Analysis) calculations (like converting camera pixels to undistorted pixels, rays, planes while carrying through the sigmas) and a collection of fits. The main fits are typical linear (weights from sigmas) and non-linear. The non-linear fits are applications/extensions of a non-linear equation solver that I plucked from Tsai. It was originally written in Fortran then machine translated to C. I covered it with C++ to make it nestable but it's the only abomination in the library. And somebody needs to understand how it works. And we need to implement it on the Jetson (I guess as a node in a neural net)

The machine vision library needs a lot more illumination modeling more camera modeling besides geometric (gains, levels, color transformations).
Illumination modeling is a huge deal, of course but, we have decent start and know how to do it. I even wrote a pretty long paper on it.

The simulator has some good pieces and PCA calculations for physics. It uses the same Pseudo Functions in the Machine Vision lib. But its a piece of Dog Doo. The basic organization might not even be right but I have a good idea of the requirements.

Appearance Modeling is a set of universal shapes. The one I use the most, I call a Nox or Noxoid. with just a few parameters it makes a six face shape from a box with rounded corners to an ellipsoid. These make scenes for Wild Magic Five game engine. Also, there's a lot of support for modeling balls and capturing ball models especially golf, tennis and baseball.

The GUI works just way I like it (so I assume you'll all want to make you own). It needs a lot more standard pieces and one final pass on graphs.



