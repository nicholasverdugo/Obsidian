
# ShapeWorks Workflow
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

![[Pasted image 20220225152626.png]]
- Open the repo in VSCode and run the file "CreateProject.py"
	- The program will prompt you first for a name for the project file.
	- Next, get the path to the shoulder data you downloaded earlier and paste it into the program. It should be that path to a folder that contains the two datasets, "Akira_Organized" and "Keisuke_Organized"
	- Next, choose which bones to import. Type 1 for scapula, 2 for humerus, or 3 for clavicle.
	- Lastly, choose the amount of bones to import. I would recommend 9 at most if you're just trying to get optimization to work correctly and mess around with parameters.
	- If you're trying to generate data for the whole dataset, however, type "all"
	- The project file will be saved in the Projects folder you created earlier
- Open ShapeWorks and click "Open Existing Project", then navigate to the Projects folder and select the file you created above to import your desired .stl files.

## Section 3: Using ShapeWorks to create point clouds
- ShapeWorks uses three steps in order to generate a point cloud:
	- *Groom*, which smooths out the models and makes them easier for the program to process
	- *Optimize*, which is what we use to actually generate the points on each mesh
	- *Analyze*, which we can use to visualize the different point clouds for each individual mesh and see how well our optimization parameters did

### Section 3a: Grooming
#### Mesh Grooming
- Mesh Grooming has three main options:
	- Fill Holes
	- Smooth
	- Remesh
- Fill holes should always be selected for our purposes, as many of the scapula meshes have...you guessed it: holes!
	- NOTE: If the grooming step is taking a long time for Scapulae, de select this option. Not recommended, but it was not completing on some computers so do this if needed.
- Smoothing is prety self explanatory: it smoothes out the mesh. This *can* be useful, but if you enable it be sure to keep iteration amounts between 1 and 3, as going higher loses the surface detail that we need to generate accurate clouds.
- Remesh modifies the mesh so that the triangles that make it up have more similar areas. This is very useful, but keep it at default. 
#### Alignment
- This section allows you to modify the alignment and the reflections of meshes
- For our purposes, the *CreateProject.py* file mentioned above assigns the "right" group to any bone that contains "Raxes" in its name. Otherwise, it assigns the "left" group
	- This is done because the Keisuke dataset is the only one with meshes from both sides of the body - Akira has only the left. The Keisuke dataset names corresponding meshes "Raxes" for the right side, and "Laxes" for the left, resulting in the above logic for the script.
- Select the "Reflect" option to reflect the meshes so that they all have the same orientation.
	- Change the "shape_file" option to "group_side" to enable this feature

![[Pasted image 20220322160529.png]]

### Section 3b: Optimizing
- There are a lot of options to choose from here, and they all have different effects. Here is what I have found through trial and error, and [the page on what the parameters mean](http://sciinstitute.github.io/ShapeWorks/workflow/optimize.html#xml-parameter-file) and some [quick tips on optimization](http://sciinstitute.github.io/ShapeWorks/workflow/optimize.html#parameter-dictionary-in-python) from ShapeWorks:
	- Number of particles
		- The amount of particles the program generates
		- Keep this number low while fine-tuning your parameters (x<64) but increase and decrease by factors of x\*2 if you need to.
		- Increasing it provides a mesh with higher detail at the end, but takes much longer
	- Initial Relative Weighting
		- How the initial correspondence is weighted
		- Increase this number (by a factor of 0.01) in order to make particles correspond better
	- Relative Weighting
		- Relative weighting of correspondence during optimization
		- Decrease this number (by a factor of 1) to help particles spread more evenly across the mesh
	- Starting Regularization & Ending Regularization
		- Don't worry about modifying these(?)
	- Iterations Per Split
		- How many times the optimization occurrs per split of points
		- Keep this around 1000, increase it to help get particles to be in the "right places" but there are diminishing returns on increasing it.
		- This affects processing time quite a lot, especially with higher amounts of particles
	- Optimization Iterations
		- How many times the program will optimize point location per iteration. Keep this around 1000.
	- Geodesic Distance
		- Don't enable this. Increases computing time by a factor of 10x and is not useful for our application. 
	- Normals
		- Tells the algorithm whether to consider surface normals and the xyz of a particle in correspondence
		- Should be enabled to avoid particles flipping sides on thin structures
		- Seems like it may be needed in order to get particles to place correctly on most smaller structures
	- Normals Strength
		- How much the algorithm ascribes strength of surface normals relative to position.
	- Procrustes
		- These options provide minimal changes and have little documentation. Would recommend to just avoid for now.
	- Multiscale Mode
		- No documentation on this option and it hasn't provided meaningful improvements. Would recommend avoiding this.
	- Multiscale Start
		- After how many particles should multiscale mode begin.
	- Narrow Band
		- Doesn't affect optimization. Don't worry about this.

### Section 3c: Exporting Data
- To export the key point data, just save the project! This will save all of the current particles in a folder with the same name as the project, just with "\_particles" appended.

![[Pasted image 20220322160701.png]]

![[Pasted image 20220322160716.png]]
