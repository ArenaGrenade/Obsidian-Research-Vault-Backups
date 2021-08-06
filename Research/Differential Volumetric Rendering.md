## Primary Contributions

## Other Literature

## Points to note

## How the method works?
- Their shape representation is done using an occupancy network, which assigns a probability to every point in space.
- The 3d surface is implicitly defined by a [[Level Sets]] defined by $f_\theta = \tau$ for some threshold parameters $\tau \in [0, 1]$. They use somethng called [[Isosurface Extraction]] for getting the surface at an arbitrary resolution.
- Thier textture representation is also very similar to the shape, but this maps to a RGB value for every point in space. It is called a texture field.
- The texture of a 3d surface is again the values over the surface, defined as above.