# Using Shapeworks Studio:
- Notes
	- Decrease number of particles (I am using 64 atm) in order to speed up optimization time so messing with settings produces faster results
	- So far, increasing relative weighting is producing the best results for correlation of points between models
- Initial relative weighting
	- "Typically `initial_relative_weighting` is selected to be small (in the order of 0.01) to enable particles to be uniformly distributed (i.e., evenly spaced) over each shape, and hence optimization starts with a good surface sampling."
- Relative weighting
	- "As we increase the `relative_weighting`, i.e., the correspondence term weight, particles tend to be distributed over surface regions that have less variability across shape samples; hence the shape distribution in the shape space tends to collapse to a single point"
- Rel. weighting at 3
![[Pasted image 20220215203442.png]]
- Rel. weighting at 10
![[Pasted image 20220215203711.png]]

	
- Using *default grooming settings* for models in the data set
	- Attempting to groom using the smooth option - poor results
- Save the project file and this note to my github branch
- Enabled normals to attempt to fix the points flipping sides - poor results
![[Pasted image 20220215204009.png]]
- Decent correspondence in some areas, but not others

![[Pasted image 20220215204201.png]]
- Decreased relative weighting - much more promising results!! Going to change it around to see its relationship with correspondence of points

![[Pasted image 20220215235151.png]]
These resulted in:
![[Pasted image 20220215235207.png]]

![[Pasted image 20220216000444.png]]
Results in
![[Pasted image 20220216000454.png]]



40%!!!!
![[Pasted image 20220216003524.png]]
![[Pasted image 20220216003531.png]]
![[Pasted image 20220216003540.png]]

Write down parameters and how they are used, and the workflow for shapeworks
Format for xlsx files to import projects as well

http://sciinstitute.github.io/ShapeWorks/use-cases/constraint-based/femur-cutting-planes.html


Avoid smoothing - not necessary 


# ShapeWorks workflow!
## Section 1: Installation
- To get ShapeWorks, first [install miniconda](https://docs.conda.io/en/latest/miniconda.html) if you don't already have it.
- Visit ShapeWorks's [GitHub](https://github.com/SCIInstitute/ShapeWorks/releases) and download the latest stable release.
- Install ShapeWorks using the installer you downloaded
- Open a miniconda terminal and navigate to the ShapeWorks folder
	- Default location is C:\\Program Files\\ShapeWorks
- Run install_shapeworks in the miniconda terminal
	- NOTE: If this step takes a while to complete, try deleting the shapeworks folder and updating your miniconda
- Once that is complete, navigate to C:\\Program Files\\ShapeWorks\\bin and run ShapeWorksStudio.exe

## Section 2: Importing a dataset
- For our purposes, we are using ShapeWorks to analyze .stl files. 
- Download our shoulder dataset from [here](https://www.dropbox.com/sh/mp15p8qoanhzwyd/AAAkmvp1IcqS9lWDwHOM2_Sna?dl=0)
- Once it is downloaded, clone the nick-dev branch from the Shoulder-Keypoint repo on GitHub and create a folder named "Projects" in the root of that folder
