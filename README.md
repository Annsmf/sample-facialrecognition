EN|[CN](README_cn.md)

Developers can deploy the application on the Atlas 200 DK to register a face, predict the face information in the video by using the camera, and compare the predicted face with the registered face to predict the most possible user.

## Prerequisites<a name="en-us_topic_0167333413_section412314183119"></a>

Before using an open source application, ensure that:

-   MindSpore Studio has been installed.
-   The Atlas 200 DK developer board has been connected to MindSpore Studio, the cross compiler has been installed, the SD card has been prepared, and basic information has been configured. 

## Software Preparation<a name="en-us_topic_0167333413_section431629175317"></a>

Before running the application, obtain the source code package and configure the environment as follows.

1.  Obtain the source code package.

    Download all the code in the sample-facialrecognition repository at  [https://github.com/Ascend/sample-facialrecognition](https://github.com/Ascend/sample-facialrecognition)  to any directory on Ubuntu Server where MindSpore Studio is located as the MindSpore Studio installation user, for example,  _/home/ascend/sample-facialrecognition_.

2.  Log in to Ubuntu Server where MindSpore Studio is located as the MindSpore Studio installation user and set the environment variable  **DDK\_HOME**.

    **vim \~/.bashrc**

    Run the following commands to add the environment variables  **DDK\_HOME**  and  **LD\_LIBRARY\_PATH**  to the last line:

    **export DDK\_HOME=/home/XXX/tools/che/ddk/ddk**

    **export LD\_LIBRARY\_PATH=$DDK\_HOME/uihost/lib**

    >![](doc/source/img/icon-note.gif) **NOTE:**   
    >-   **XXX**  indicates the MindSpore Studio installation user, and  **/home/XXX/tools**  indicates the default installation path of the DDK.  
    >-   If the environment variables have been added, skip this step.  

    Enter  **:wq!**  to save and exit.

    Run the following command for the environment variable to take effect:

    **source \~/.bashrc**


## Deployment<a name="en-us_topic_0167333413_section1675513165418"></a>

1.  Access the root directory where the facial recognition application code is located as the MindSpore Studio installation user, for example,  **_/home/ascend/sample-facialrecognition_**.
2.  <a name="en-us_topic_0167333413_li08019112542"></a>Run the deployment script to prepare the project environment, including compiling and deploying the ascenddk public library, downloading the network model, and configuring Presenter Server. The Presenter Server is used to receive the data sent by the application and display the result through the browser.

    **bash deploy.sh** _host\_ip_ **internet**

    -   _host\_ip_: this parameter indicates the IP address of the Atlas 200 DK developer board.


    Example command:

    **bash deploy.sh 192.168.1.2 internet**

    -   When the message  **Please choose one to show the presenter in browser\(default: 127.0.0.1\):**  is displayed, enter the IP address used for accessing the Presenter Server service in the browser. Generally, the IP address is the IP address for accessing the MindSpore Studio service.
    -   When the message  **Please input a absolute path to storage facial recognition data:**  is displayed, enter the path for storing face registration data and parsing data in MindSpore Studio. The MindSpore Studio user must have the read and write permissions. If the path does not exist, the script will automatically create it.

    Select the IP address used by the browser to access the Presenter Server service in  **Current environment valid ip list**  and enter the path for storing facial recognition data, as shown in  [Figure 1](#en-us_topic_0167333413_fig184321447181017).

    **Figure  1**  Project deployment<a name="en-us_topic_0167333413_fig184321447181017"></a>  
    ![](doc/source/img/project-deployment.png "project-deployment")

3.  Start Presenter Server.

    Run the following command to start the Presenter Server program of the facial recognition application in the background:

    **python3 presenterserver/presenter\_server.py --app facial\_recognition &**

    >![](doc/source/img/icon-note.gif) **NOTE:**   
    >**presenter\_server.py**  is located in the  **presenterserver**  directory. You can run the  **python3 presenter\_server.py -h**  or  **python3 presenter\_server.py --help**  command in this directory to view the usage method of  **presenter\_server.py**.  

    [Figure 2](#en-us_topic_0167333413_fig69531305324)  shows that the presenter\_server service is started successfully.

    **Figure  2**  Starting the Presenter Server process<a name="en-us_topic_0167333413_fig69531305324"></a>  
    ![](doc/source/img/starting-the-presenter-server-process.png "starting-the-presenter-server-process")

    Use the URL shown in the preceding figure to log in to Presenter Server \(only the Chrome browser is supported\). The IP address is that entered in  [2](#en-us_topic_0167333413_li08019112542)  and the default port number is  **7009**. The following figure indicates that Presenter Server is started successfully.

    **Figure  3**  Home page<a name="en-us_topic_0167333413_fig64391558352"></a>  
    ![](doc/source/img/home-page.png "home-page")

    **Figure 4** Example IP Address <a name="en-us_topic_0167333823_fig64391558353"></a>  
    ![](doc/source/img/connect.png "Example IP Address")

    Among them:
    - The IP address of the  Atlas 200 DK developer board is 192.168.1.2 (connected in USB mode).
    - The IP address used by the Presenter Server to communicate with the Atlas 200 DK is in the same network segment as the IP address of the Atlas 200 DK on the UI Host server. For example: 192.168.1.223.
    - The following is an example of accessing the IP address of the Presenter Server using a browser: 10.10.0.1, because the Presenter Server and MindSpore Studio are deployed on the same server, the IP address is also the IP address for accessing the MindSpre Studio through the browser.


## Running<a name="en-us_topic_0167333413_section833124235913"></a>

1.  Run the facial recognition application.

    Run the following command in the  **sample-facialrecognition**  directory to start the facial recognition application:

    **bash run\_facialrecognitionapp.sh** _host\_ip_ _presenter\_view\_app\_name camera\_channel\_name_  &

    -   _host\_ip_: For the Atlas 200 DK developer board, this parameter indicates the IP address of the developer board.
    -   _presenter\_view\_app\_name_: Indicates  **App Name**  displayed on the Presenter Server page, which is user-defined. The value of this parameter must be unique on the Presenter Server page.
    -   _camera\_channel\_name_: Indicates the channel to which a camera belongs. The value can be  **Channel-1**  or  **Channel-2**. For details, see  **View the Channel to Which a Camera Belongs** of [Atlas 200 DK User Guide](https://ascend.huawei.com/documentation).
    
    Example command:

    **bash run\_facialrecognitionapp.sh 192.168.1.2 video Channel-1 &**

2.  Use the URL that is displayed when you start the Presenter Server service to log in to the Presenter Server website \(only the Chrome browser is supported\).

    [Figure 4](#en-us_topic_0167333413_fig1189774382115)  shows the Presenter Server page.

    **Figure  5**  Presenter Server page<a name="en-us_topic_0167333413_fig1189774382115"></a>  
    ![](doc/source/img/presenter-server-page.png "presenter-server-page")

    >![](doc/source/img/icon-note.gif) **NOTE:**   
    >-   The Presenter Server of the facial recognition application supports a maximum of two channels at the same time , each  _presenter\_view\_app\_name_  corresponds to a channel.  
    >-   Due to hardware limitations, the maximum frame rate supported by each channel is 20fps,  a lower frame rate is automatically used when the network bandwidth is low.  

3.  Register a face.
    1.  Click the  **Face Library**  tab and enter a user name in the  **Username**  text box.

        ![](doc/source/img/en-us_image_0167333516.png)

    2.  Click  **Browse**  to upload a face image. Crop the face image based on the ratio of  **Example Photo**.

    1.  Click  **Submit**. If the upload fails, you can change the cropping ratio.

4.  Perform facial recognition and comparison.

    On the  **App List**  tab page, click  _video_  for example in the  **App Name**  column. If a face is displayed in the camera and matches the registered face, the name and similarity information of the person are displayed.


## Follow-up Operations<a name="en-us_topic_0167333413_section1092612277429"></a>

-   **Stopping the Facial Recognition Application**

    The facial recognition application is running continuously after being executed. To stop it, perform the following operation:

    Run the following command in the  **sample-facialrecognition**  directory as the MindSpore Studio installation user:

    **bash stop\_facialrecognitionapp.sh** _host\_ip_

    _host\_ip_: For the Atlas 200 DK developer board, this parameter indicates the IP address of the developer board.

    Example command:

    **bash stop\_facialrecognitionapp.sh 192.168.1.2**

-   **Stopping the Presenter Server Service**

    The Presenter Server service is always in the running state after being started. To stop the Presenter Server service of the facial recognition application, perform the following operations:

    Run the following command to check the process of the Presenter Server service corresponding to the facial recognition application as the MindSpore Studio installation user:

    **ps -ef | grep presenter | grep facial\_recognition**

    ```
    ascend@ascend-HP-ProDesk-600-G4-PCI-MT:~/sample-facialrecognition$ ps -ef | grep presenter | grep facial_recognition
    ascend 22294 20313 22 14:45 pts/24?? 00:00:01 python3 presenterserver/presenter_server.py --app facial_recognition
    ```

    In the preceding information,  _22294_  indicates the process ID of the Presenter Server service corresponding to the facial recognition application.

    To stop the service, run the following command:

    **kill -9** _22294_
