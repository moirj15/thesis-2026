Modern GPUs are capable of calculating massive volumes of single precision
floating point data, allowing for efficient rendering of highly complex scenes.
However, objects in these scenes must remain within a certain distance to the
origin of the scene in order to maintain their positions without experiencing
jittering due to floating point error during transformation calculations. This
limitation doesn’t affect smaller scenes, but are quite pronounced when
rendering a scene at the scale of the Earth or the solar system.

There are a number of techniques that combat this to certain degrees of success:
performing the model-view-projection transformation using double precision
floating point computations on the CPU before uploading to GPU memory for the
final rasterization and pixel shading, computing the model-view-projection
matrix using double precision on the CPU before converting to single precision
and uploading to GPU memory for vertex transformations, using a camera centered
origin when transforming vertices, using native double precision computations on
the GPU, and using emulated double computations on the GPU to achieve near
double precision. However, use of these techniques can result in the loss of
performance, increase memory usage, or fail to remove jitter in large enough
scenes.

I propose a new technique that aims to mitigate all of these issues. This
technique requires the position and orientation of a bounding box in the scene
to be known ahead of time. With this information the object is then rendered to
three textures with three different view points, with each view point mapping to
one of the three exposed surfaces of the bounding box. The vertices of the
bounding box are then transformed using emulated double calculations on the GPU,
before being textured with the previously mentioned textures. 

This new technique will be evaluated against the previously mentioned techniques
in two ways: performance and removal of jitter. To achieve this, an application
will be created using C++ and D3D11, implementing all of the techniques
described. This application will collect the relevant timing data from the CPU
and GPU to evaluate performance. It will also determine how successful each
technique is at removing jitter by comparing 60 rendered frames against a
control set of frames. The control set will be generated using the technique of
double precision model-view-projection transformations on the CPU.

