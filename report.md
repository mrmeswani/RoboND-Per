## Project: Perception Pick & Place

---




[//]: # (Image References)


[p1]: ./Images/test1_world.png
[p2]: ./Images/test2_world.png
[p3]: ./Images/test3_world.png
[ex]: ./Images/ex.png
[m1-1]: ./Images/mat1_test1-1.png
[m1-2]: ./Images/mat1_test1-2.png
[m2-1]: ./Images/mat1_test2-1.png
[m2-2]: ./Images/mat1_test2-2.png
[m3-1]: ./Images/mat1_test3-1.png
[m3-2]: ./Images/mat1_test3-2.png
[raw]: ./Images/raw.png
[stat_filter]: ./Images/statistical_filter.png
[voxel]: ./Images/voxel.png
[passthru_filter]: ./Images/passthru_filter.png
[cluster_bad]: ./Images/cluster_bad.png
[cluster]: ./Images/cluster.png


### Exercise 1, 2 and 3 pipeline implemented
#### 1. Complete Exercise 1 steps. Pipeline for filtering and RANSAC plane fitting implemented.

#### 2. Complete Exercise 2 steps: Pipeline including clustering for segmentation implemented.  

#### 2. Complete Exercise 3 Steps.  Features extracted and SVM trained.  Object recognition implemented.

I completed all three exercises and was able to locate all the objects as shown below
![alt text][ex]

### Pick and Place Setup

#### 1. For all three tabletop setups (`test*.world`), perform object recognition, then read in respective pick list (`pick_list_*.yaml`). Next construct the messages that would comprise a valid `PickPlace` request output them to `.yaml` format.

I tested all three worlds and was able to get 3/3 in test1.world, 5/5 in test2.world and 7/8 i test3.world 
I have include the output images below and also included the output yaml file in ./Output folder
![alt text][p1]
![alt text][p2]
![alt text][p3]

### Discussion 
In each of the steps below I incrementally added them in my pipeline and viewed the output on RViz and tried to optimize each step before adding more in my pipeline.

   
* First thing was to remove some of the noise using statistical filtering, upon applying the filter the image improved quite a bit as shown. I would have liked to do some more filtering, but for now this is as far as I could get. 
![alt][stat_filter]

* Next I set my VOXEL size to 0.01 and the image is shown below. I found that making it any smaller did not change the accuracy. However, I did not try to increase the sizes and hence it is likely that a larger size could have also worked.
![alt][voxel]

* For the passthrough filter initally I had done only Z direction filtering. But during clustering i noticed that it was trying to detect clusters even for the drop boxes. So I decided to filter in the horizontal axis as well, which captured only the table and removed the detection of the dropboxes. Shown below is the result of clustering in all axis.  
![alt][passthru_filter]  

* For RANSAC filtering I found that distance 0.007 was reasonable. I did not try to tune this parameter too much beyond what I had done for the exercises. 

* Clustering is the time I spent the most in fine tuning. I found that my biscuits were getting clustered as two different different biscuit objects, as shown below, when I set min cluster size to 30. 
![alt][cluster_bad]

From the above image, some part of the biscuit has a lighter color, and so by setting cluster min size to >=100 I was able to tune it out. Setting min size too high say 300 was not good for other smaller objects were not being detected. After some adjustment i found 100 for min size worked well for all three test worlds and the result is shown below. 
![alt][cluster]  

* For classification we had to first train and generate the models. In my training i did 10 poses per object and I tried different SVC kernels, but I found that with linear kernel, C=0.01, tol < 1e-5 was giving my > 90% accuracy on all test worlds. The remaining models were not quite as effective. If I had more time I would probably explore training models and data to get 8/8 on test3_world.  The confusion matrices (both raw and normalized) for all tests is shown below. 
![alt][m1-1] 
![alt][m1-2]
![alt][m2-1] 
![alt][m2-2]  
![alt][m3-1] 
![alt][m3-2]
 
* After the cluster was completed I did output my resulting images as shown in the begining of this report.  

* I completed the generation of output*,yaml file, taking care to just generate data if there was a match between pick_list and my detected objects.   

If I had more time the first thing I would do is improve my object detection to get 100% accuracy on test3_world, next I would have to liked to get the robot to actually pick and place my detected object.

