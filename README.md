# Class project for BUSN33200: Artificial Intelligence
### Worth the Surf: Real Time Prediction for the Perfect Conditions for a Solo Surf
pulls surf quality ratings (optimalScore) from surfline.com and 
cross references with Roboflow photo archive of images from surfline cams

[sharable link for model](https://teachablemachine.withgoogle.com/models/cbAMLqL_P/)

### Problem statement:
There is a fundamental problem that every surfer faces. When the waves are good, the beach is usually crowded. In responding to this assignment, I set out to design an algorithm that could identify those rare moments when the waves are good, and the crowds are slim. This algorithm has the following elements:
input: image from a Surfline.com beach camera
output: the surf is good, and there are less than two surfers in the water
user: any surfer who loves the waves and hates the crowds (which is roughly all surfers)
benefit: once the system identifies that the surf is good, and there are few surfers in the water, an application could send a push notification to a user, notifying them that it is a good time to paddle out

### Data collection and methodology:
The primary source for all data included in this project is Surfline.com. I collected (1) images from Surfline.com surf cams, via Roboflow.com, and (2) ratings of the quality of surf conditions from the Surfline.com API. Both types of data are used for classification, but the algorithm only sees the images. The methodology for classification and collection is further documented in this repository. 
The data were collected under the following conditions: 

    •	Surf cam images: Surfline.com shared a data set of thousands of images with Roboflow for use in their Surfer Spotting project. Roboflow modified these images to be 224x224, applied augmentations to zoom and other aesthetics, and made a dataset of ~40K .jpg files available for download. 
    •	Surf quality ratings for a given beach: I wrote a few proof of concept scripts for the collection of quality ratings (called ‘optimalScore’ ratings and assessed from 0 to 2) from the Surfline.com API. However, this data set is prospective, and historical data is not available through the API. So, for this project, I manually assigned optimalScore ratings to a subset of images, using a qualitative methodology. The outcome of this labeling is available here.   
    •	Data Input: I took a random sample of 100 images classified by Roboflow to contain no surfers, and a random sample of 100 images classified by Roboflow as containing surfers. The metadata for the second set of images indicated the number of surfers per image. I used this to classify images as crowded or not-crowded as defined by whether Roboflow had identified > 2 surfers in an image. Then, I manually rated images as 0 or 2 based on my judgement of wave quality. 
    
The data is representative of the information that the algorithm will encounter in deployment because it is an actual subset of the Surfline.com surf cam data. It is not representative, in that I elected to manually assign optimalScore ratings in the absence of historical forecasts. In practice, I would want to re-collect both data sets (i.e. images and optimalScore ratings) then use the Roboflow Surfer Spotting model, which is available for download on their site, along with my API query to construct a larger data set that more accurately represents the optimalScores calculated by Surfline.com

### Performance:
The algorithm agreed (i.e. assigned > 50% to the label) with the labeling in 15 out of 19 cases, or 79% of the time. Importantly, of the 4 cases in which the model was incorrect, it never erroneously assigned an image the label of ‘good_not_crowded.’ My hypothesis as to what drove the errors in classification is that a great deal of the uncertainty in the model comes from asking the algorithm to identify both surf quality and crowdedness in the same model. In production, this could be resolved with a decision tree which uses Surfline.com API data to identify surf quality and the Roboflow Surfer Spotter to identify crowdedness. 

Finally, with a fair result and this vanishingly small data set, I am heartened about the model. Given the use case (informing users that it might be the perfect time for a surf), I would argue that false positives are more costly than false negatives. And given the absence of false negatives on this data set, I would be optimistic about future iterations. 
