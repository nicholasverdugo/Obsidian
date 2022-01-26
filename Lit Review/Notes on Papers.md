# Literature Review Notes

## List of papers
- Banks 96
- Lafortune 92
- Mahfouz 05
- Myers 12


### Banks 1996 - Accurate Measurement of Three-Dimensional Knee Replacement Kinematics Using Single-Plane Fluoroscopy
- TKR kinematics measurement needs an alternative to skin markers, as those don't fully show the small movements occurring between bones.
- It is best to attempt to measure *in vivo* kinematics as synthetic simulations (*in vitro*) have doubt about whether they are actually correct, or account for all movement happening in the joint. 
- Fluoroscopy creates an opportunity for good measurements as it doesn't interfere with TKR implants, doesn't require markers, and is cheap, efficient, and simple compared to CT scans and MRI.
- The method described in this paper uses little computing power (compared to making a "model and match" technique, where the kinematics would be achieved through intensive six-degree searches) to achieve pose estimates due to using an efficient method of 2-D object recognition and extending that into 3-D pose estimation. 
- Principally, the measurement approach utilizes fluoroscopy to obtain a sequence of silhouette images of the knee as it moves. These silhouettes are processed to create an outline, which is compared to a precomputed library of shapes and allows the program to choose the best match, and return a pose for that frame. 
- The outline (contour) is represented with a series of Normalized Fourier Descriptors. These are found via selection of a centroid for the specific pose, and creating a periodic function via tracing distances from this centroid to the contour, advancing counter-clockwise with constant increments. 
- This approach removes four degrees of freedom from the object without affecting the shape of the contour itself.