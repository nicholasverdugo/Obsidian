# Literature Review Notes

## List of papers
- Banks 96
- Lafortune 92
- Mahfouz 05
- Myers 12
- Burton 21


### Banks 1996 - Accurate Measurement of Three-Dimensional Knee Replacement Kinematics Using Single-Plane Fluoroscopy
- TKR kinematics measurement needs an alternative to skin markers, as those don't fully show the small movements occurring between bones.
- It is best to attempt to measure *in vivo* kinematics as synthetic simulations (*in vitro*) have doubt about whether they are actually correct, or account for all movement happening in the joint. 
- Fluoroscopy creates an opportunity for good measurements as it doesn't interfere with TKR implants, doesn't require markers, and is cheap, efficient, and simple compared to CT scans and MRI.
- The method described in this paper uses little computing power (compared to making a "model and match" technique, where the kinematics would be achieved through intensive six-degree searches) to achieve pose estimates due to using an efficient method of 2-D object recognition and extending that into 3-D pose estimation. 
- Principally, the measurement approach utilizes fluoroscopy to obtain a sequence of silhouette images of the knee as it moves. These silhouettes are processed to create an outline, which is compared to a precomputed library of shapes and allows the program to choose the best match, and return a pose for that frame. 
- The outline (contour) is represented with a series of Normalized Fourier Descriptors. These are found via selection of a centroid for the specific pose, and creating a periodic function via tracing distances from this centroid to the contour, advancing counter-clockwise with constant increments. 
- This approach removes four degrees of freedom from the object without affecting the shape of the contour itself.
- A "Shape Library" is created using this method with contours based on known, standardized positionings of a model of the prosthesis. The computed NFDs are compared to the library's NFDs, and the program interpolates between the two best matches to find a resulting pose. This is called an "interpolated NFD", which the program uses to successively  recalculate a new interpolated NFD with the goal of minimizing the difference between this interpolated NFD and the inputted NFD. This results in an NFD as close to the input as possible, along with calculated pose coordinates based on the library poses. 
- This allows for a program to dynamically generate high resolution poses without needing an infinitely extensive library to store all potential poses. 
- Distortions in images must be accounted for and a consistent frame of reference must be established in order to compare generated NFDs to synthetically created NFDs.

## Burton 2021 - Automatic tracking of healthy joint kinematics from stereo-radiography sequences
- 