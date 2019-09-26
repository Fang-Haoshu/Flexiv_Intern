### Notes:
1. Make sure that the operating system is consistent with the libraries in 3rd_party/lib/
2. Libraries Requested: gflags, mujoco, pcl, vcg
  * gflags: -lgflags
  * mujoco: -lmujoco150 -lGL include libmujoco150.so libmujoco150nogl.so in the lib directory
  * pcl: 
    [Reference](https://askubuntu.com/questions/916260/how-to-install-point-cloud-library-v1-8-pcl-1-8-0-on-ubuntu-16-04-2-lts-for)
    
    Remember to install libboost,libeigen, VTK in advance
    
    Include -lpcl_common -lpcl_kdtree -lpcl_registration -lpcl_search in the lib directory
    
  * vcg: include directory to the vcg lib
3. Compile main.cpp
```
make
```
4. Run main.o to calculate the transformation matrix
```
./main.o --mesh-file dirsctory to the stl file
```
