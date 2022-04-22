# Geographic Coordinates

GSTools provides support for
[geographic coordinates](https://en.wikipedia.org/wiki/Geographic_coordinate_system)
given by:

- latitude `lat`: specifies the north–south position of a point on the Earth's surface
- longitude `lon`: specifies the east–west position of a point on the Earth's surface

If you want to use this feature for field generation or Kriging, you
have to set up a geographical covariance Model by setting `latlon=True`
in your desired model (see `CovModel`):

```python
import numpy as np
import gstools as gs

model = gs.Gaussian(latlon=True, var=2, len_scale=np.pi / 16)
```

By doing so, the model will use the associated `Yadrenko` model on a sphere
(see [here](https://onlinelibrary.wiley.com/doi/abs/10.1002/sta4.84)).
The `len_scale` is given in radians to scale the arc-length.
In order to have a more meaningful length scale, one can use the `rescale`
argument:

```python
import gstools as gs

model = gs.Gaussian(latlon=True, var=2, len_scale=500, rescale=gs.EARTH_RADIUS)
```

Then `len_scale` can be interpreted as given in km.

A `Yadrenko` model $ C $ is derived from a valid
isotropic covariance model in 3D $ C_{3D} $ by the following relation:

$C(\zeta)=C_{3D}\left(2 \cdot \sin\left(\frac{\zeta}{2}\right)\right)$

Where $ \zeta $ is the
[great-circle distance](https://en.wikipedia.org/wiki/Great-circle_distance).

## Note
`lat` and `lon` are given in degree, whereas the great-circle distance
$ zeta $ is given in radians.

Note, that $ 2 \cdot \sin\left(\frac{\zeta}{2}\right) $ is the
[chordal distance](https://en.wikipedia.org/wiki/Chord_(geometry))
of two points on a sphere, which means we simply think of the earth surface
as a sphere, that is cut out of the surrounding three dimensional space,
when using the `Yadrenko` model.

## Note
Anisotropy is not available with the geographical models, since their
geometry is not euclidean. When passing values for `CovModel.anis`
or `CovModel.angles`, they will be ignored.

Since the Yadrenko model comes from a 3D model, the model dimension will
be 3 (see `CovModel.dim`) but the `field_dim` will be 2 in this case
(see `CovModel.field_dim`).
