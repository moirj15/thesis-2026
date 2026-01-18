# Abstract


# Introduction

## Outline
- Touch on GPU floating point primitive for vertex transformations
- Touch on single precision floating point accuracy
- Touch on jittering as a result of loss of floating point accuracy
- Bring up cases where jittering can be problematic
  - i.e. Scales of the problamatic scenes

## Rough Draft


- GPUs provide excellent performance for processing massive amounts of floating point calculations with single precision
- Sufficient, so long as objects aren't too large and they're within a certain distance of each other in a scene
- Quickly becomes an issue when dealing with objects for globe based systems or solar system scales
- Jittering becomes apparent when zooming in on an object or when observing an object far from the origin
- 

# Background/Related Work

## Outline
- Touch on how there's been multiple attempts to solve this

- Full Double Calculation on CPU
- Relative to Center
- Relative to Eye CPU and GPU
- Emulated Doubles on GPU

## Rough Draft

# Hypothesis

## Outline

## Rough Draft


# Solution Design and Implementation

## Outline

## Rough Draft


# Roadmap

## Outline

## Rough Draft