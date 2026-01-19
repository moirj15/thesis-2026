# Abstract


# Introduction

## Outline
- Touch on GPU floating point primitive for vertex transformations
- Touch on single precision floating point accuracy
- Touch on jittering as a result of loss of floating point accuracy
- Bring up cases where jittering can be problematic
  - i.e. Scales of the problematic scenes

## Rough Draft

- GPUs can calculate massive amounts of floating point calculations
- Allows 3D applications to process large amounts of vertices
- Single precision floating point is the most common
- Provides enough precision for a variety of scenes
- Falls flat once scenes become large enough
  - globe
  - Solar system
- Large scenes result in jittering
- While native double precision is present on some hardware, it's not ubiquitous
  - Not supported in Metal and some older hardware
  - Also performs at 1/4 rate due to the number of ALUs available for doubles
- There are a number of solutions for this with varying tradeoffs and levels of success
- Rest of the proposal provides background of existing techniques, as well as a new technique which balances the tradeoffs
  - High level description of new technique


<!-- - GPUs provide excellent performance for processing massive amounts of floating point calculations with single precision
- Sufficient, so long as objects aren't too large and they're within a certain distance of each other in a scene
- Quickly becomes an issue when dealing with objects for globe based systems or solar system scales
- Jittering becomes apparent when zooming in on an object or when observing an object far from the origin
-  -->

# Background/Related Work

## Outline
- Touch on how there's been multiple attempts to solve this

- Full Double Calculation on CPU

- Relative to Center
- Relative to Eye CPU and GPU
- Emulated Doubles on GPU

## Rough Draft

- There have been a number of techniques developed to combat this.
- These include:
  - Performing the vertex transformation using double precision on the GPU
  - Relative to Center
  - Relative to Eye
  - Emulated double GPU implementation

### Performing the vertex transformation using double precision on the GPU
- By far the most straight forward approach
- Illuminates jittering completely due to the precision provided
- Has the obvious drawback of losing the performance provided by the GPU for massively parallel calculations

### Relative to Center
- Requires calculating the center point of the object via the average of the positions
- the center is then subtracted from each position
- Then a special form of the Model-View matrix is constructed
  - First transform the Center by the Model-View matrix, Center_eye
  - Replace the translation component of the Model-View matrix with Center_eye to get the Model-ViewRTC
  - Calculate MVPrtc by P * MVrtc
  - Convert to 32-bit floats before uploading
- Models from most modeling packages are already in this format, so not necessary in those cases. Unless the model is exceedingly large
- More useful for large objects
- Needs to be calculated per object
  - But only needs to be recomputed if the camera's position changes or if the center position of an object changes
  - Extra overhead for dynamic objects, better for static
- Not great for large objects, as objects will be too far away for 32-bit precision
- Specifically, objects spanning more than 131,071 meters per Ohlarik
- 

- Also need to note that this and relative to eye work by making the distances smaller in the final matrix multiplication, staying within the accurate bounds

### Relative to Eye
- Similar to RTC, RTE modifies the usual MVP so the matrix values are small enough to be accurate when using 32-bit floats
- First the normal MV is calculated using doubles, but the translation portion is zeroed out
- Then the camera's position is subtracted from the position component of the vertices using double precision and uploaded to a dynamic vertex buffer 
  - Results in having to reupload whenever a viewer or object moves
  - Thankfully if a single object moves then only that object's vertices have to be updated
  - The same MVP can be used for all models in the scene, assuming they're in the same coordinate system
- This approach has the drawback of requiring a large number of updates to a GPU buffer, effectively limiting object size by PCIE bandwidth
- There's another version of this technique that takes advantage of the GPU
- First step is the same as the prior version, create a MV with the zeroed out translation
- Then the positions, as doubles, are stored in a static buffer as two floats
  - Show math for this calculation
- Then the GPU can be used to perform the subtraction
  - Show math
- Leads to much faster calculations and removes the update drawbacks from the CPU method
- Maximum distance per Ohlarik that this provides 1cm accuracy is 549,755,748,352m
- Perfect for Earth and nearby planets
- but falls flat once we reach Jupiter
- Also has the drawback of not resolving jittering when zoomed super close up

### Emulated double GPU implementation


# Solution/Hypothesis

## Outline
- Describe the combination of parallax texture rendering and emulated doubles
- Touch on prior art with parallax rendering for voxels in that one youtuber and Teardown
- Need some diagrams to describe the parallax rendering

## Rough Draft


# Solution Design and Implementation

## Outline
- Need to implement control + other methods described
- Will be using C++ and Directx11
- Use Assimp for importing models
- Glm for math
- Dear Imgui for simple gui
- Describe high-level flow for each method I'm testing against
  - Flow chart?
- Common base scene format
  - Slightly modified for each example 
- Comparison to existing methods
  - Collection of relevent timing data
  - Comparison of jitter against a control

## Rough Draft


# Roadmap

- End of February: Application complete
- End of March: Data collected and first draft completed
- End of April: Final draft complete
- May: Thesis defense


# Papers
- Deron Ohlarik, Precision, Precisions: https://help.agi.com/STKComponents/html/BlogPrecisionsPrecisions.htm
  - Maybe use thorne's paper/thesis instead
- Thorne's PHD thesis chapter on jittering
- Emulated doubles paper
- More up to date doubles paper (pay $45 D:< )
- 3D Globe book, chapter on vertex transform precision
- 