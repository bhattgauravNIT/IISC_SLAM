# IISC_SLAM

This is Gaurav Bhatt an engineering student from NATIONAL INSTITUTE OF TECHNOLOGY SRINAGAR and intern under IISC BANGLORE, INDIA .I hope this file will be helpful for you to implement live streaming of Large scale direct monocula
slam without any error on your consoles .

# INSTRUCTIONS 

LSD-SLAM method uses direct SLAM algorithm to perform simultaneous localization(tracking) of
the agent or robot alongside with 3d mapping of the topology of the environment in which it is
being made to move on real time CPU.

[ https://www.youtube.com/watch?v=R1njsqpt-MA&t=226s ] ------ live streaming uav lab IISC  
[ https://www.youtube.com/watch?v=6KRlwqubLIU ] ----------- LSD-SLAM machine dataset  

 # RELATED PAPERS  

1.IISC Banglore research paper yet to be published.  
2. LSD-SLAM: Large-Scale Direct Monocular SLAM - Computer Vision.  
https://vision.in.tum.de/_media/spezial/bib/engel14eccv.pdf

 # PREREQUISTIES  

1. We have tested LSD-SLAM on ubuntu 14.04 ros indigo . (However it can be performed on any
higer version of ubuntu as well).    
1.1 ROS indigo set up ------- [ http://wiki.ros.org/indigo/Installation/Ubuntu ].  

follow the step by step procedure provided by the link.   

 # PROCEDURE

Open up an console and follow.
  
Copy the iisc_ws folder to your desired location say Desktop using the github link . This
workspace contains all necessary folder which are being made error free for compilation . A
usb_cam package is provided for live streaming using your camera ( certain changes in launch file
need to be done in case you are not using your laptop head camera i.e any other external camera like
logitec 720p say in case , in the 3 rd line value need to be changed to 1(for external camera) and 0
(for head camera ) ).

$ sudo apt-get install python-rosinstall  
$ cd iisc_ws  
$ rosws init . /opt/ros/indigo  
$ rosws set ~/iisc_ws/iiscslam_dir -t.  
$ echo "source ~/iisc_ws/setup.bash" >> ~/.bashrc   
$ bash  
$ cd iiscslam_dir  
$ sudo apt-get install ros-indigo-libg2o ros-indigo-cv-bridge liblapack-dev libblas-dev freeglut3-dev libqglviewer-dev libsuitesparse-dev    
$ rosmake lsd_slam   
$ rosmake usb_cam   

 # TESTING LSD_SLAM FOR DATASETS-

Taking to consideration that yu have succesfully build your lsd_slam package without any building
errors you are now ready to test lsd_slam for datasets....

1. [ https://vision.in.tum.de/research/vslam/lsdslam?redirect=1 ] this link provides datasets in
video as well as pointcloud type involving both png as well as bag files . Ex. Desk sequence ,
Machine sequence, bottledata etc.  
2. [ http://vmcremers8.informatik.tu-muenchen.de/lsd/LSD_room.bag.zip ] for direct download of
LSD_room.bag.zip file . Download and extract this dataset if yu wish to use this.  
  
# If you are using LSD_machine.bag dataset or any other dataset of (.bag) format then in seprate terminals.

$ roscore  
$ rosrun lsd_slam_viewer viewer  
$ rosrun lsd_slam_core live_slam image:=/image_raw camera_info:=/camera_info  
$ rosbag play ~/LSD_machine.bag  

# For using any .png format dataset i.e a collection of large number of images then in sepreate terminals.

$ roscore  
$ rosrun lsd_slam_viewer viewer  
$ rosrun lsd_slam_core dataset_slam _files:=<files> _hz:=0 _calib:=<calibration_file>  
 
i.e we simply need to write the complete path to dataset file and to that of camera calibration file in which camera calibrated parameters are present ex..   

rosrun lsd_slam_core dataset_slam_files:=/home/gaurav/Desktop/LSD_machine/images/ hz:=0 calib:=/home/gaurav/Desktop/LSD_machine/cameracalibration.cfg  

 # LSD SLAM ON LIVE STREAM------------------------
 
Taking into consideration that yu have successfully calibrated your camera which yu will be using
to perform live lsd_slam and has obtained its calibartion file . For more details consider

[ http://wiki.ros.org/camera_calibration/Tutorials/MonocularCalibration ]    
  
If you are using pinhole model for camera calibration than the file obtained will be in (. yaml)
format with co-efficents of distortion matrix and camera matrix. In order to use this file we need to
convert it into format of files basically selfwrite it in formats of files as shown in.  
  
........./lsd_slam/lsd_slam_core/calib/ anyone of them however not that of atan_calib.cfg format as
we have earlier used pinhole model for camera calibration and its parameters values will be
different than atan format models.  

Headcamera.yaml ( obtained calibration file)  
  
Camera_matrix is of format                      data:     [ f(x) 0 c(x) 0 f(y) c(y) 0 0 1 ]   
Distortion_coffecients is of format             data:     [ k1 k2 p1 p2 ]  
  
We have converted or created a opencv camera model format file using our head_camera.yaml file
however any format could be preffered according to your wish howevefr format of that file must be
known .OPENCV MODEL FORMAT----------------

f(x) f(y) c(x) c(y) k1 k2 p1 p2   
inputWidth inputHeight  
"crop" / "full" / "none" / "e1 e2 e3 e4 0"   
outputWidth outputHeight     
  
Or any other format according to your camera calibration type . Above input height and width of
image being viewed by camera say 640x480 in our case . The third line specifies how the image is
distorted, either by specifying a desired camera matrix in the same format as the first four intrinsic
parameters, or by specifying "crop", which crops the image to maximal size while including only
valid image pixels.  

$ roscore  
$ rosrun lsd_slam_viewer viewer  
$ rosrun usb_cam usb_cam_node  
$ rosrun lsd_slam_core live_slam /image:=<yourstreamtopic> _calib:=<calibration_file>  
 
here your image topic and path to your camera calibration file need to be provided ex.

rosrun lsd_slam_core live_slam /image:=/usb_cam/image_raw
_calib:=/home/Desktop/my_calibr.cgf

**Tracking immediately diverges / I keep getting "TRACKING LOST for frame 34 (0.00% good
Points, which is -nan% of available points, DIVERGED)!"**

- double-check your camera calibration.   
- try more translational movement and less roational movement.  

Some tracking lost problem is common while using handheld cameras however is not encountered
during working with bebop quadcopter live streaming due to its stability of motion and accurate
setted keyframes of it's inbuild camera.  
