## Project: Perception Pick & Place

---




[//]: # (Image References)


[p1]: ./Images/test1_world.png
[p2]: ./Images/test2_world.png
[p3]: ./Images/test3_world.png
[ex]: ./Images/ex.png

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

   
1. First off in the detection pipeline i played with Voxel size, it did not seem to matter too much and I decided on 0.01 LEAF SIZE which I had also used in the exercises.  
2.  For the passthrough filter I decided to filter in all axis to reduce extra noise. I found that the clustering was getting affected by the dropboxes, so horizontal filter in X and Y direction helped remove that noise.   
3. For RANSAC filtering I found that distance 0.007 was reasonable.  
4. Clustering is the time I spent the most in fine tuning. I found that cluster size was the most dominant factor and by adjusting them I was able to get quite good accuracy.  
5. For classification we had to first train and generate the models. In my training i did 10 poses per object and I tried different SVC kernels, but I found that with linear kernel, C=0.01, tol < 1e-5 was giving my > 90% accuracy on all test worlds. The remaining models were not quite as effective. If I had more time I would probably explore training models and data to get 8/8 on test3_world.   
6. After the cluster was completed I did output my results as shown above.  
7. I completed the generation of output*,yaml file, taking care to just generate data if there was a match between pick_list and my detected objects.   

If I had more time the first thing I would do is improve my object detection to get 100% accuracy on test3_world, next I would have to liked to get the robot to actually pick and place my detected object.

