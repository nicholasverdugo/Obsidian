# 7/26 
Broke some power outlets and had to move the pc to the side wall :^)
Added a new helper function to replace repeated code:
/*If Viewing Inverted Images Update*/
	if (ui.image_list_widget->currentIndex().row() >= 0 && ui.inverted_image_radio_button->isChecked() == true) {
		if (ui.camera_A_radio_button->isChecked()) {
			matToVTK(loaded_frames[ui.image_list_widget->currentIndex().row()].GetInvertedImage(), current_background);
		}
		else {
			matToVTK(loaded_frames_B[ui.image_list_widget->currentIndex().row()].GetInvertedImage(), current_background);
		}
		ui.qvtk_widget->update();
	}
	/*If Viewing Edge Images Update*/
	else if (ui.image_list_widget->currentIndex().row() >= 0 && ui.edges_image_radio_button->isChecked() == true) {
		if (ui.camera_A_radio_button->isChecked()) {
			matToVTK(loaded_frames[ui.image_list_widget->currentIndex().row()].GetEdgeImage(), current_background);
		}
		else {
			matToVTK(loaded_frames_B[ui.image_list_widget->currentIndex().row()].GetEdgeImage(), current_background);
		}
		ui.qvtk_widget->update();
	}
	/*If Viewing Dilated Images Update*/
	else if (ui.image_list_widget->currentIndex().row() >= 0 && ui.dilation_image_radio_button->isChecked() == true) {
		if (ui.camera_A_radio_button->isChecked()) {
			matToVTK(loaded_frames[ui.image_list_widget->currentIndex().row()].GetDilationImage(), current_background);
		}
		else {
			matToVTK(loaded_frames_B[ui.image_list_widget->currentIndex().row()].GetDilationImage(), current_background);
		}
		ui.qvtk_widget->update();
	}

the function is named "updateImages()", located at line 283. Copy-pasted the above code and made a void function to take its place, appears at lines 1150 and 1188.

load order is as follows: calibration file->images->models

//this function replaces the below lines, up to "load jit model" - this also occurs on lines 1429, 1613, and 1779.
	createSegments("FemHR");

/*Pose Estimate Progress and Label Visible*/
	ui.pose_progress->setValue(5);
	ui.pose_progress->setVisible(1);
	ui.pose_label->setText("Initializing high resolution segmentation...");
	ui.pose_label->setVisible(1);
	ui.qvtk_widget->update();
	qApp->processEvents();
//this switch was added to account for the two different types of potential uses for the function - if you need to un-do, remove the case and preserve the "this" line
	/*Segment*/
	switch (segType) {
	case "FemHR":
		this->on_actionSegment_FemHR_triggered();
		break;
	case "TibHR":
		this->on_actionSegment_TibHR_triggered();
		break;
	}

	unsigned int input_height = 1024;
	unsigned int input_width = 1024;
	unsigned int orig_height = loaded_frames[0].GetInvertedImage().rows;
	unsigned int orig_width = loaded_frames[0].GetInvertedImage().cols;
	unsigned char* host_image = (unsigned char*)malloc(input_width * input_height * sizeof(unsigned char));
	ui.pose_label->setText("Initializing STL model on GPU...");
	ui.qvtk_widget->update();

	/*STL Information*/
	vector<vector<float>> triangle_information;
	QModelIndexList selected = ui.model_list_widget->selectionModel()->selectedRows();
	stl_reader_BIG::readAnySTL(QString::fromStdString(loaded_models[selected[0].row()].file_location_), triangle_information);

	/*GPU Models for the current Model*/
	gpu_cost_function::GPUModel* gpu_mod = new gpu_cost_function::GPUModel("model", true, orig_height, orig_width, 0, false, // switched cols and rows because the stored image is inverted?
		&(triangle_information[0])[0], &(triangle_information[1])[0], triangle_information[0].size() / 9, calibration_file_.camera_A_principal_); // BACKFACE CULLING APPEARS TO BE GIVING ERRORS

	ui.pose_progress->setValue(55);
//this switch was added to account for the two different types of potential uses for the function - if you need to un-do, remove the case and preserve the line setting the text on the ui label
	switch (segType) {
	case "FemHR":
		ui.pose_label->setText("Initializing femoral implant pose estimation...");
		break;
	case "TibHR":
		ui.pose_label->setText("Initializing tibial implant pose estimation...");
		break;
	}
	ui.qvtk_widget->update();

Removed the alternative algorithm for estimation, and the algorithm 2 for estimation.

# 7/28

To-do for today:
1. Get JTAutoGPU compiling again - DONE
2. Build libtorch using CMake - In progress
3. Continue refactoring

Lines 1434 - Linker error occurring potentially because of definitions missing or .lib files not being found

Working on getting everything up to date on the drive, and compiling - DONE, compiled fully

Libtorch guide is WIP on master branch

Need to figure out how to get ninja to be recognized by conda so that python setup.py install can be run
https://github.com/pytorch/pytorch
https://github.com/pytorch/pytorch/blob/master/docs/source/notes/windows.rst#building-from-source


# 7/30

To-do for today:
1. Finish building libtorch with CMake - Done?
2. Continue refactoring

Doesn't look like there's going to be a meeting, doing homework before starting lab stuff

Homework is done

Got pytorch to build! deleted the cmakecache.txt file to force the compiler to use ninja to compile


# 8/10

To-do for today:
1. Continue refactoring
2. Fix libtorch include messiness
3. Sign up for ClickUp - DONE

ClickUp Stuff
List =>Status => Task

May need to take a look at includes/code for torch::jit::Modules to see why that error is occurring
also 

# 8/11

To-do for today:
1. Build a pytorch GUI using C++ to pass models in

Steps for the software:
1. Load in the image
2. Pass through the model
3. The output of the model is the segmentation
4. Highlight wherever the knee is (display the output)

Notes:
-Forward passing through the model is where the code breaks for everyone
-**CUDA is needed to send stuff to the GPU
-**Things are stored as tensors in Torch
-Tensor: a matrix that is specially made to work with the GPU
-Pretty much anything can be used to display the image


Error(s):
entry point cudnnSetRNNProjectionLayers could not be located in the DLL torch_cuda.dll - FIXED - correct DLLs were placed in the debug folder, from the libtorch download

# 8/12

example-app was built, To-do for next time:
use opencv to display an image (image.tif) before segmentation, and then segment it, then display after
need to use libtorch to create a tensor from the tif file, then pass that into the module.forward function.
Point CMake to OpenCVConfig.cmake, may need to redownload a fresh version of OpenCV from github.

