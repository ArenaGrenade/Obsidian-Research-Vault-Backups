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

## New things learnt
- Zero-level Set

## How their method works?
