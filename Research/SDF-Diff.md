## Primary Contriubtions
- Differentially Renderer based on SDFs that supports ML.
- Mulit-view reconstruction using shape optimization. Involves use of Multi-resolution with gradient descent optimiation.
- Single-vew 3d reconstruction without 3d supervision.
## Other Literature
- [[DeepSDF]] paper talks about implicit representation.
- [[Deep Level Sets]] is an end-to-end model predicting implicit surfaces.
- [[SMVS]] aka Shading-aware multi-view synthesis can capture all the scene parameters from an image itself.
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
- The training is performed greedily. If loss of a target view is greater than the average of previous training iteration, then that sample is updated a maximum of 20 times or until loss lowers below average. For all other target views, we update the SDF lattice 5 times.
- The updates are stopped in any iteration if the loss starts increasing.
- The stopping criterion for the optimization is tolerance of loss and small step length.
- The multi-resolution method is proposed to incrementally increase the lattice resolution and train parallely. This seems to give them a better / faster convergence for high-frequency details.
- Their render resolution is also dependent on the current lattice resolution.

### Single-View
- They use ShapeNet Dataset.
- 3d IoU used as reconstriction metric.
- They built the network in parts - a encoder + decoder for a course SDF construction module and another that refines this output and generates finer SDFs.
- They use the above loss function with an additional laplacian loss which is __????????????????????????????__
- 