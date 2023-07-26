# Forehead Wrinkle Matching

## Introduction
Authentication using facial features becomes a task with the ongoing pandemic which requires us to wear a mask.  The technique used in this experiment does not require expensive scans, rather makes use of images captured from a standard hand held mobile phone.

## Database
The data was collected by the students enrolled in the course CS-F425 Deep Learning (BITS Pilani).  Data was collected in 2 sessions with a minimum of 12 hour gap between the sessions.  Each session captured 10 pictures, 5 close up and 5 far off.  From the total data collected, 336 images were used for the experiment.  For 28 subjects, there were 3 close up images and 3 far off images per session.

## System Pipeline
The general recognition pipeline consists of RoI (Region of Interest) extraction, image enhancement, feature extraction and image matching.  Various feature extraction algorithms are used and the score obtained from them are used to plot the genuine/imposter histogram.  A ROC (receiver operating characteristic curve) is plotted at varying classification thresholds.  At the corresponding threshold, the EER (equal error rate) and CRR (correct recognition rate) is calculated.

### Image pre-processing (RoI extraction, Image enhancement)
The region of interest was extracted manually by letting the the data provider align their head such a way that only the forehead wrinkles were in the image.  The images were converted to grayscale and Contrast Limited Adaptive Histogram Equalization(CLAHE) was applied.  CLAHE is better than the general histogram equilization.  CLAHE is a variant of Adaptive histogramequalization (AHE) which takes care of over-amplification of the contrast.  CLAHE operates on small regions in the image, called tiles, rather than the entire image.  The neighboring tiles are then combined using bilinear interpolation to remove the artificial boundaries.

### Feature Extraction and Score Calculation
The  feature  extractors  used  in  this  experiment  are  SIFT,  SURF,  ORB,  AKAZE,  BRISK,  ArcFace  and  SSIM.  The  first  four algorithms are used to get the local image features for the input image.  To perform image comparison, the extracted featurevectors are compared for the given two images.  These matches are computed using the Brute Force algorithm which compares the feature descriptors of the two images under consideration.
Lowe’s ratio test is performed to remove the ambiguous matches.  Each keypoint of the first image is matched with a numberof keypoints from the second image.  The 2 best matches for each keypoint (best matches is the ones with the smallest distance measurement) are kept.  Lowe’s test checks that the two distances are sufficiently different.  If they are not, then the keypoint is eliminated and will not be used for further calculations.
ArcFace is a neural network classification model.  It uses a similarity learning mechanism that allows distance metric learning to be solved in the classification task by introducing Angular Margin Loss to replace Softmax Loss.  The distance between faces is calculated using cosine distance, which is calculated by the inner product of two normalized vectors.
SSIM stands for Structural Similarity Index.  It is a metric that quantifies the structural difference between two images.  This metric depends on the image data and not the features present in the image.

## Results
ArcFace performs the best with the given data. The results are good since it used pre-trained weights and parameters to extract facial features from the image.  The similarity scores calculated were 1-(cosine distance between two images). SSIM also performed relatively well due to the similarity in position and lighting of the images comapared. Since SSIM does not extractthe features, rather compares the image data the way it is, it is not reliable with varying camera angles, lighting, contrast, etc. Since we performed appropriate image enhancements, the lighting and contrast of the compared images were similar. The reason for the large number of 0s in the histograms of ORB, AKAZE and BRISK is due to the lack of features extracted by the feature extractor.  This is due to the presence of blurry images in the dataset. The Contrast Limited Adaptive Histogram Equalization gave marginally better results for all the feature extractors than regular equilization.

![Results Table](/table.png "Results table")
![ROC curves](/roc.png "ROC curves")
