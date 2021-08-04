## Primary Contriubtions
- Differentially Renderer based on SDFs that supports ML.
- Mulit-view reconstruction using shape optimization. Involves use of Multi-resolution with gradient descent optimiation.
- Single-vew 3d reconstruction without 3d supervision.
## Other Literature
- [[DeepSDF]] paper talks about implicit representation.
- [[Deep Level Sets]] is an end-to-end model predicting implicit surfaces.
## Points to note
- Rendering is not invertible so we can only optimize for close results.
- The paper handles only shape optimizations, ig this means we can primarily only do [[Shape Reconstruction]] and other parameters are known.
- Differentiable rendering needs to provide derrivatives of image wrt the scene parameters themselves. This is used when calculating the derrivatives of the loss function for any optimization.
- Any model built on this must generate a 3d lattice / manifold of SDF values in a discrete fashion.
## New things learnt
### Zero-level Set of a function
It is....

## How their method works?
- Usage of discrete SDF. Trilinear interp for building continuous SDF. The object surface would be the zero level set of this.
- We essentially apply a 8-point discrete sphere tracing. So sample values at these 8 points are non-differentiable. but, pixel color is defined on the local set of SDF samples using an automatiic differentiaion framework.
- This would mean that the we calculate distances and shading parameters in a non-differentiable fashion. But the actual shading happens in a differentiable way.
- Inputs to the shading is lighting parameters, camera parameters and nearby lattice samples of SDF and provides the intersection point as well as the normals at this point.
- The intersection point is calculated using __????????????????????????????????????__
- The normals at the point are then calculated by calculating the gradients of the SDF at the lattice vertices using [[Central Finite Differencing]] and then trilinearly interpolating them to the point. The normals expression would depend on a 4 x 4 x 4 neighborhood around the intersection point.
- They use a simplistic diffuse shader using the calculated parameters.
### Multi-view
- They generated training set by taking ~26 camera orientations.
- Initialized the SDF to that of a sphere.
- The loss/energy function is essentially the sum of the L_2 loss of the shape-differentiiable image render and the target views, and a regularization loss which essentially ensures the gradients to have unit magnitude.
- The regularization loss is implemented to be finite diifferentials of the individual SDF points and summed over the SDF lattice.