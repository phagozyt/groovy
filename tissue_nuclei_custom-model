//This code is written in groovy and used in QuPath
//You MUST adapt the file directories to your location
//This code is made to be used for H&E whole slide tissue 
//Before using: You have 
//1) created a Pixel Classifier to detect your tissue (name in line 14)
//2) created an Object Classifier to detect your cells of interest (name in line 58)
//enjoy and let me know if there are issues alina.gavrilov@yahoo.de

setImageType('BRIGHTFIELD_H_E');
setColorDeconvolutionStains('{"Name" : "H&E default", "Stain 1" : "Hematoxylin", "Values 1" : "0.65111 0.70119 0.29049", "Stain 2" : "Eosin", "Values 2" : "0.2159 0.8012 0.5581", "Background" : " 255 255 255"}');
resetSelection();

//HERE enter YOUR created pixelclassifier name
createAnnotationsFromPixelClassifier("nameof_your_createdPixelclassifier", 1000000.0, 0.0, "DELETE_EXISTING")

//Delete first Annotation (which is unclassified)
def annotations = getAnnotationObjects()
selectObjects(annotations[0])
clearSelectedObjects();

selectAnnotations()

import qupath.ext.stardist.StarDist2D
import qupath.lib.scripting.QP
 
// IMPORTANT! Replace this with the path to your StarDist model
// that takes 3 channel RGB as input (e.g. he_heavy_augment.pb)
// You can find some at https://github.com/qupath/models
// (Check credit & reuse info before downloading) https://github.com/qupath/models/tree/main/stardist
def modelPath = "YOURdirectoryOFStarDistmodel/he_heavy_augment.pb"

// Customize how the StarDist detection should be applied
// Here some reasonable default options are specified
def stardist = StarDist2D
    .builder(modelPath)
    .normalizePercentiles(1, 99) // Percentile normalization
    .threshold(0.5)              // Probability (detection) threshold
    .pixelSize(0.5)              // Resolution for detection
    .measureShape()              // Add shape measurements
    .measureIntensity()          // Add nucleus measurements
    .build()
	 
// Define which objects will be used as the 'parents' for detection
// Use QP.getAnnotationObjects() if you want to use all annotations, rather than selected objects
def pathObjects = QP.getSelectedObjects()
 
// Run detection for the selected objects
def imageData = QP.getCurrentImageData()
if (pathObjects.isEmpty()) {
    QP.getLogger().error("No parent objects are selected!")
    return
}
stardist.detectObjects(imageData, pathObjects)
stardist.close() // This can help clean up & regain memory
println('Done!')

//HERE enter YOUR created object classifier name
runObjectClassifier("nameof_your_createdobjectclassifier");

