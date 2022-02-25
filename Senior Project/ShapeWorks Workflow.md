
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
![[Screenshot 2022-02-23 121208.png]]
- Open the repo in VSCode and run the file "CreateProject.py"
	- The program will prompt you first for a name for the project file.
	- Next, get the path to the shoulder data you downloaded earlier and paste it into the program. It should be that path to a folder that contains the two datasets, "Akira_Organized" and "Keisuke_Organized"
	- Next, choose which bones to import. Type 1 for scapula, 2 for humerus, or 3 for clavicle.
	- Lastly, choose the amount of bones to import. I would recommend 9 at most if you're just trying to get optimization to work correctly and mess around with parameters.
	- The project file will be saved in the Projects folder you created earlier
- Open ShapeWorks and click "Open Existing Project", then navigate to the Projects folder and select the file you created above to import your desired .stl files.

## Section 3: Using ShapeWorks to create point clouds
- ShapeWorks uses three steps in order to generate a point cloud:
	- *Groom*, which smooths out the models and makes them easier for the program to process
	- *Optimize*, which is what we use to actually generate the points on each mesh
	- *Analyze*, which we can use to visualize the different point clouds for each individual mesh and see how well our optimization parameters did

### Section 3a: Grooming
- Grooming has three main options:
	- Fill Holes
	- Smooth
	- Remesh
- Fill holes should always be selected for our purposes, this also speeds up grooming by quite a bit
- Smooth should not be selected, as it creates a mesh that loses much of the surface detail we need in this use case
	- WindowedSinc with 1 iteration produces good results. Look into this more!
- Remesh is what we are going to be messing with the most, but it seems like the default settings work well so far.

### Section 3b: Optimizing
- There are a lot of options to choose from here, and they all have different effects. Here is what I have found through trial and error, and [the page on what the parameters mean](http://sciinstitute.github.io/ShapeWorks/workflow/optimize.html#xml-parameter-file) and some [quick tips on optimization](http://sciinstitute.github.io/ShapeWorks/workflow/optimize.html#parameter-dictionary-in-python) from ShapeWorks:
	- Number of particles
		- The amount of particles the program generates
		- Keep this number low while fine-tuning your parameters (x<64)
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
		- Don't modify this. Not useful.
	- Multiscale Start
		- Don't modify this. Not useful.
	- Narrow Band
		- Doesn't affect optimization. Don't worry about this.

### Section 3c: Exporting Data
- To export the key point data, click file->export->Export current particles
![[Pasted image 20220223150915.png]]