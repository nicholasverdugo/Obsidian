3/22

Working on reflecting the Left-facing meshes in the Keisuke dataset because ShapeWorks is dumb
	Cloned the shapeworks env and am updating its version of python to 3.9 in order to use some code syntax that is present in the *CreateProject.py* file
	This didn't work hahaha just gonna change the syntax to work with 3.7
	CreateProject is now working as intended!
	Now, it will *reflect* the "raxes" files to match the orientation of the others, and save those reflections in a "reflect" folder in the dataset.
	This can be done on an unmodified dataset, so no need to reupload
If it isn't finished today, make sure to tell Diana (or just do it myself lol) to flip the "Laxes" particles across the x axis (should be this axis, as ShapeWorks is flipping the .stls across this axis)
This is done in order to realign them to their original fluoroscopy images. We're reflecting them just to create uniform keypoints that also correlate.
