# Understanding MediaPipe's Face Mesh Output

This repository is intended to serve as a collection of resources for
understanding the output of [MediaPipe's Face Mesh](https://google.github.io/mediapipe/solutions/face_mesh.html)
models.

## Background

MediaPipe Face Mesh is a solution that estimates the position of face landmarks
for given input images.
It returns a list of canonical length and order contained one 3D coordinate for
each face landmark.
While each coordinate at the same index in the returned list always represents
the same landmark the official documentation is very sparse on what each
landmark encodes.

## Landmarks visualized

The following images illustrate the semantic of each coordinate index, by
(1) showing detected face landmarks drawn on-top of a [reference face image](https://purepng.com/photo/12072/people-man-face-hero),
(2) showing the same face landmarks without the reference face image, and
(3) showing face landmarks positioned according to the UV coordinates of a
texture for the face mesh.

Each image is available both with labeled and unlabeled landmark indices.

Additionally, we also show the detected landmarks both for the regular Face
Mesh model and also the refined one that is available as
`faceMesh.setOptions({ refineLandmarks: true });` in JavaScript.
The refined model shares the meaning of the first 468 landmark indices with the
regular model (although their position is usually estimated to be at slightly
different coordinates), but also adds 10 additional landmarks about left and
right eye irises.

The code to generate these images is available in [index.html](./index.html) and
the accompanying JavaScript files.

| Name                        | Unnumbered                                                                                         | Numbered                                                                                                   |
| --------------------------- | -------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| reference face image        | [![face.png](https://i.imgur.com/nJtujExl.jpg)](https://i.imgur.com/nJtujEx.png)                   |                                                                                                            |
| landmarks with face         | [![face-landmarks.png](https://i.imgur.com/hLJRSJNl.jpg)](https://i.imgur.com/hLJRSJN.png)         | [![face-landmarks-numbered.png](https://i.imgur.com/uEwvTVjl.jpg)](https://i.imgur.com/uEwvTVj.png)        |
| refined landmarks with face | [![face-refined-landmarks.png](https://i.imgur.com/KqZ6z9ul.jpg)](https://i.imgur.com/KqZ6z9u.png) | [![face-refined-landmarks-numbered.pn](https://i.imgur.com/BIMVncRl.jpg)](https://i.imgur.com/BIMVncR.png) |
| landmarks                   | [![landmarks.png](https://i.imgur.com/gjJw7vNl.jpg)](https://i.imgur.com/gjJw7vN.png)              | [![landmarks-numbered.png](https://i.imgur.com/EHFWkYnl.jpg)](https://i.imgur.com/EHFWkYn.png)             |
| refined landmarks           | [![refined-landmarks.png](https://i.imgur.com/GEIKZ1dl.jpg)](https://i.imgur.com/GEIKZ1d.png)      | [![refined-landmarks-numbered.png](https://i.imgur.com/hKwI04al.jpg)](https://i.imgur.com/hKwI04a.png)     |
| uv coordinates              | [![uvCoords.png](https://i.imgur.com/JQXBI9Il.jpg)](https://i.imgur.com/JQXBI9I.png)               | [![uvCoords-numbered.png](https://i.imgur.com/MQ0wXzIl.jpg)](https://i.imgur.com/MQ0wXzI.png)              |

Sadly, no UV coordinates seem to be available for the iris landmarks of the
refined model.
Therefore, there is no UV coordinates visualization available for the refined
model.

## Further resources

- The MediaPipe project provides an [official UV coordinate visualization](https://github.com/google/mediapipe/blob/63e679d99ca45b30514a9d84c9351a2d77bb9ba0/mediapipe/modules/face_geometry/data/canonical_face_model_uv_visualization.png)
  that is similar ours.
  However, the official one is of low resolution and the numbers of landmark
  indices are hard to read.
  Note that the official one uses a tesselation different to ours.
- The MediaPipe project provides canonical 3D face models in [FBX](https://github.com/google/mediapipe/blob/63e679d99ca45b30514a9d84c9351a2d77bb9ba0/mediapipe/modules/face_geometry/data/canonical_face_model.fbx)
  and [OBJ](https://github.com/google/mediapipe/blob/63e679d99ca45b30514a9d84c9351a2d77bb9ba0/mediapipe/modules/face_geometry/data/canonical_face_model.obj)
  format.
  However, these 3D models seem to be outdated as the order of vertices does not
  match the returned facial landmarks by current face mesh models.
  These models use the same tesselation as the official UV coordinate
  visualization that differs to ours.
- The tesselation used by us is the one available in the `FACEMESH_TESSELATION`
  constant of [@mediapipe/face_mesh](https://cdn.jsdelivr.net/npm/@mediapipe/face_mesh@0.4.1633559619/face_mesh.min.js).
  Sadly, this package only seems to be available in minified form to the public.
  The same tesselation is used in the [current face mesh Python package](https://github.com/google/mediapipe/blob/63e679d99ca45b30514a9d84c9351a2d77bb9ba0/mediapipe/python/solutions/face_mesh_connections.py#L67-L493).
- The UV coordinates used by us are from [`uv_coords.ts` of the tfjs-models
  repository](https://github.com/tensorflow/tfjs-models/blob/838611c02f51159afdd77469ce67f0e26b7bbb23/face-landmarks-detection/src/mediapipe-facemesh/uv_coords.ts).
  Note that this is a link to an older commit.
  Sadly, no current official repository seems to provide these UV coordinates.
- The most in-depth programmatic description of landmark indices is available
  in [`keypoints.ts` of the tfjs-models repository](https://github.com/tensorflow/tfjs-models/blob/838611c02f51159afdd77469ce67f0e26b7bbb23/face-landmarks-detection/src/mediapipe-facemesh/keypoints.ts).
  Note that as in the point above this links to an older commit of a
  since-removed file.
