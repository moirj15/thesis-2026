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

### Relative to Eye

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
