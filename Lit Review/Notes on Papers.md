# Literature Review Notes

## List of papers
- Banks 96
- Lafortune 92
- Mahfouz 05
- Myers 12
- Burton 21

## What should be in the Literature Review?
- After completion, the review should have:
	- Explained the major achievements in the field
	- Reviewed the main areas of debate
	- Identified outstanding research questions

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
-  This method builds on older techniques, passing *biplanar* radiographs through convolutional neural networks to generate kinematics for the tibia-fibula pair, the femur, and the patella. 
- Using biplanar radiographs provides another frame of reference to generate more accurate and detailed kinematics.
- The generated results were compared to manually created kinematics and showed strong potential for the new framework.
- The framework utilizes four steps: 
	- first, a CNN chooses which frames can be tracked through the rest of the framework.
		- this essentially ensures that all three bones that are tracked (tibia-fibula, patella, and femur) are visible and trackable in both planes
	- second, another CNN generates segmented versions of these frames and semantic key points that will be used for tracking
	- third, a Perspective-n-Point problem is used to estimate the poses of each bone based on the key points generated in the previous step
	- lastly, the pose estimates are used to minimize differences in appearance between the segmented images and the projections of the bones for both planes in the radiographic image
- This method uses a technique named *pyramidal grid search* to refine poses. This utililzes a down-scaled version of each radiograph, beginning at 64x64, and evaluates all search points. This is repeated, with the image's resolution doubling each step. This ensures that the algorithm quickly rules out large batches of search space that don't need to be checked.

 ## Zhu 2012 - The accuracy and repeatability of an automatic 2D-3D fluoroscopic image-model registration technique for determining shoulder joint kinematics
 - This method utilizes two fluoroscopes to create a common imaging zone of the shoulder. 
 - Next, 3-D models of the scapula and humerus are created using MR images of the patient's shoulder. A point cloud of key points is created from these MR images, and used to generate a local coordinate system for the shoulder. 
 - This point cloud and coordinate system are imported into a virtual dual fluoroscopic image system (DFIS), which was set up to mirror the real-world equivalent used to capture the initial images of the patient's shoulder. 
 - The digital DFIS is used to project outlines of the shoulder onto the fluoroscopic images, and via manipulation of the positions of the joint, the original shoulder position is reproduced with 6DOF (x,y,z, alpha,beta,gamma) 
	 - To achieve this, an initial "seed" pose is required to be inputted manually. Once this is supplied, the algorithm can sequentially move through each DFIS pair, using the previous pose as the seed for the current.
	 - 20 random poses are generated from the seed pose (with constraints to translation being up to 5mm in any direction and rotation up to 5 degrees in any direction) and optimized to minimize the distance from the outline of the model and its projection. Once this is completed for all 20 poses, the average is taken and this generates the final optimized pose.
 - 