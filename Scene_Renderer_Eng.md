### Install Unreal Engine
1. Register an [Epic Games](www.epicgames.com) account.
2. Log in [Unreal Engine](www.unrealengine.com). Click PERSONAL under the account and select CONNECTED ACCOUNTS. Connect your github account.
3. Install Unreal Engine from source code. The project used edition 4.20. The installation step is as：
   ```
   git clone https://github.com/EpicGames/UnrealEngine -b 4.20
   cd UnrealEngine
   ```
4. Modify Ln294 of UnrealEngine/Engine/Source/Programs/UnrealBuildTool/Configuration/ModuleRules.cs as:

   `
   294:		public bool bUseRTTI = true;
   `
5. Insert 'typename between two commas at Ln326 of 'UnrealEngine/Engine/Source/Runtime/RenderCore/Public的RenderingThread.h' as:

   `
   ENQUEUE_UNIQUE_RENDER_COMMAND_ONEPARAMETER_DECLARE_OPTTYPENAME(
   TypeName,ParamType1,ParamName1,ParamValue1,typename,Code)
   `
6. Set up the sources and build.
   ```
   ./Setup.sh
   ./GenerateProjectFiles.sh
   make
   ```
   Note： As the minimum system requirements for UE4 include 8 GB RAM, you may leave only one terminal window open during the building
   process so that the system won't crush.
 
 ### Start with Scene Renderer
 In UnrealEngine directory，
   ```
   cd Engine/Binaries/Linux/
   ./UE4Editor "/home/yigu/Simulator/projs/scene_renderer/scene_renderer.uproject"
   ```
 If it raises the error that .so （such as: libmujoco150nogl.so）cannot be found and iso couldn't compile, then run 
 ```
 export PATH=Simulator/projs/scene_renderer/ThirdParty/mujoco/lib
 ```
 to add the lib path.
 
 ### Run Scene Renderer
 Click Play. Reset the directory as: 
 
 ![Step 3](Step3.png)
 
 Issues may be encountered:
 1. Check that mjkey.txt suits the current computer and is not out-dated. You could apply for a new license at [mujoco](https://www.roboti.us/license.html).
 2. If error that mjkey.txt couldn't be found raises although it exists, modify the location of mjkey.txt in the following way.
 
    Click BluePrints -> Open Level Blueprint:
    
    ![Step 1](https://github.com/renxinyang/Flexiv_Intern/blob/master/Step1.png)
    
    Modify the location in the blank of Key File：
    
    ![Step 2](Step2.png)
 3. Update protoco:
     1.  Install [grpc](https://github.com/grpc/grpc/blob/master/BUILDING.md) from source:
     ```
     git clone -b $(curl -L https://grpc.io/release) https://github.com/grpc/grpc
     cd grpc
     git submodule update --init
     make
     ```
     2. In Source/scene_renderer/Private/RecompileProto.py, update the location of GRPC_PLUGIN_PATH at Ln118 and PROTOC at Ln120.
     3. Run RecompileProto.py 
     4. Replace file in ~/Simulator/projs/dataset_generator with the newly generated SceneRenderer_pb2.py and SceneRenderer_pb2_grpc.py in rotobuf_py.
     5. Upgrade grpc， protobuf to the latest version
     ```
     pip3 uninstall grpcio
    pip3 uninstall protobuf
    pip3 install --user grpcio
    pip3 install --user protobuf
    ```
    Note:
    Use /usr/bin/python instead of the one in virtual environment such as anaconda.
    python with which the source code is built is outdated. Functions in packages such as scipy may be deprecated.
### Run Simulator/projs/dataset_generator/robot_control_kine.py
```
sudo python robot_control_kine.py
```
