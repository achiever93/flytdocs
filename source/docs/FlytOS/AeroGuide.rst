.. _aero_guide:


FlytOS Installation guide for Intel Aero Compute Board
======================================================

Preparing your Intel Aero Board
-------------------------------

.. _install_dependencies_aero:

Installing FlytOS dependencies
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. caution:: Intel Aero Compute board currently supports only the Yocto Linux distribution. Since FlytOS is only supported on Ubuntu/Debian based Linux distros, you will have to re-flash the operating system. This may void your Aero Board's warranty so we recommend users to use  their discretion while installing FlytOS on the board. Flytbase does not take any responsibility and is free from any liability caused by following these instructions to install Ubuntu on Intel Aero Board.

1. List of FlytOS dependencies to be installed in your Flight Computer:

   a) Linux - Ubuntu 16.04. First create an Ubuntu 16.04 bootable USB drive by following `these instructions <https://www.ubuntu.com/download/desktop/create-a-usb-stick-on-ubuntu>`_ . Connect the USB drive to the Aero Board using a micro USB OTG cable. You may also want to use a USB hub to attach a keyboard and mouse to the Board. Then power up the Aero board and press escape while it boots up to enter the BIOS menu. Select the option to boot from your USB drive. Then follow `these instructions <https://www.ubuntu.com/download/desktop/install-ubuntu-desktop>`_ to install Ubuntu on the board. 
   
   b) `ROS - Kinetic <http://wiki.ros.org/kinetic/Installation/Ubuntu>`_ (install *ros-kinetic-desktop* package)
   
   c) `OpenCV 2.4 <http://docs.opencv.org/2.4/doc/tutorials/introduction/linux_install/linux_install.html>`_ (for vision/video streaming APIs).
   d) Other dependencies - To install run the following commands in your terminal.

   .. literalinclude:: include/flytos_dependency.sh
      :language: bash   
 
.. 2. You have to update some kernel modules for video streaming to work properly. Run the following script as root or run each command with sudo permission.
   
..    .. literalinclude:: include/kernel_module_update.sh
..       :language:  bash  

2. Before proceeding further, add the following lines at the end of your /etc/bash.bashrc file. To open the file for editing, run the following command the terminal ``sudo nano /etc/bash.bashrc`` and to save your edited file, press ``ctrl+o+ENTER`` and to exit press ``ctrl+x``.

   .. code-block:: bash
   
       source /opt/ros/kinetic/setup.bash
       export PYTHONPATH=$PYTHONPATH:/flyt/flytapps:/flyt/userapps
       source /flyt/flytos/flytcore/setup.bash

Installing FlytOS debian package
--------------------------------

.. note:: This step requires you to have a registered FlytBase Account. In case you don't have an account, :ref:`create a FlytBase Account<create_flytbase_account>` before you proceed. 

Next, you **MUST update your FlytOS debian package** by following the steps below:

1. `Login <http://my.flytbase.com>`_ to your FlytBase Account.
2. Download the hardware specific `FlytOS Debian Package <http://my.flytbase.com/FlytOS>`_ from your FlytBase Account.
3. Verify that the dependencies are installed. To install run the following commands in your terminal.

   .. literalinclude:: include/flytos_dependency.sh
      :language: bash	

4. Once you have downloaded the Debian package, run the following command in your terminal to install FlytOS: 
   
.. code-block:: bash
   
   #make sure to provide absolute path of the debian package file: /home/flytpod/flytos_*.deb
   $ sudo apt install -y <path to debian package location>/flytos_*.deb 

4. Check for **Congratulations! FlytOS installation completed** message at the end.
5. Just in case you see any dependency issues cropping up in your screen while installing FlytOS, kindly run the following command and execute the previous command again:
   
.. code-block:: bash
   
   $ sudo apt -f -y install

.. caution:: You must :ref:`activate your device<activate_flytos_aero>`, without which critical APIs would not function.


.. _flytos_basics_aero:


FlytOS Basics
-------------

**Start/Stop FlytOS on boot**

1. If you are using FlytOS Linux image, FlytOS starts automatically on bootup.
2. On bootup FlytOS will also check for any updates. Available updates will be downloaded and installed automatically.
3. You can find more information on FlytOS automatic updates :ref:`here<flytos_updates>`.

**Start/Stop FlytOS from command line**

1. Launch FlytOS
       
   Once you have installed FlytOS, you are ready to build your own apps. If you have flashed FlytOS Linux Image, FlytOS would be launched automatically at every system bootup.

   To launch FlytOS, open a **new** terminal and run this command.

   .. code-block:: bash
       
       $ sudo $(rospack find core_api)/scripts/launch_flytOS.sh

   .. important:: If you get this error: ``Error: package 'core_api' not found``, source your /etc/bash.bashrc file.
	

2. Kill FlytOS
       
   To kill this instance of FlytOS, run this command in your terminal. 

   .. code-block:: bash
       
      $ sudo $(rospack find core_api)/scripts/stop_flytOS.sh    
       

.. **Security and Authentication**

.. From a Security and Authentication perspective, following layers are considered:


.. 1. Secure WiFi network using WPA2:
..    This is achieved by setting up a secure WiFi network (on FlytPOD by default or on a ground router).
.. 2. SSL (https and wss) encryption:
..    FlytOS uses SSL certificates and secure protocols (https, wss).
.. 3. User and Request authentication:
..    The last point involves, authenticating a user and providing role based access via a login mechanism. It also includes authenticating all the FlytAPIs for which a token based authentication mechanism is used.

.. **Accessing built-in apps with FlytOS**

.. 1. Open your browser and go to the following link - ``http://<ip-address-of-device>/flytconsole``.
.. 2. Enter ``flytpod`` in place of IP address in case you are connected to FlytPOD in AP mode- ``http://flytpod/flytconsole``.


.. 3. You will be directed to a page that shows a warning **Connection is not private**. FlytOS contains self signed SSL certificates to enable access over local network.
   
       
..    .. image:: /_static/Images/fOSinst1.png
..       :align: center
.. 4. Bypass the warning by clicking Advanced> Proceed to localhost. Confirm adding an exception if prompted to do so.
.. 5. Next you will be directed to FlytOS login page. Login using the default credentials provided to you.
       
..    .. image:: /_static/Images/fOSinst2.png
..       :align: center
.. 6. Once you have logged in you will see the list of standard apps along with other settings.
       
..    .. image:: /_static/Images/fOSinst3.png
..       :align: center

.. When a user tries to access an onboard web app e.g. FlytConsole, a login page is served asking for user credentials. The user credentials are validated and home page for the app is served. The response of a login request contains a token. All the FlytAPI calls need to have this token in the http header otherwise the request fails with unauthorized error.

.. The user authentication follows Single Sign On approach with a common login for FlytPOD allowing access to all the onboard apps.


.. **FlytAdmin for User Administration**
   
.. There is an inbuilt app FlytAdmin for user administration. Only ‘admin’ users have access to this app. The FlytOS admins of a device will be able to add, activate, edit, delete, deactivate users for that device using this app. The app provides views for Users and Roles. 

.. .. image:: /_static/Images/fOSinst4.png
..    :align: center

.. .. image:: /_static/Images/fOSinst5.png
..    :align: center


.. _activate_flytos_aero:

Activate FlytOS
---------------

.. note:: This step requires you to have a registered FlytBase Account. In case you don't have an account, :ref:`create a FlytBase Account<create_flytbase_account>` before you proceed.

You have to activate installed FlytOS, without which critical APIs would not function.

1. Make sure your Flight Computer has internet access before proceeding.
2. :ref:`Launch FlytConsole <FlytConsole_launch>` and click on **Activate Now tag** under **License tab** at bottom right corner. A popup will appear which will direct you to the device registration page. If you are not logged in, enter your FlytBase Account credentials to log in.
3. Choose a device nick-name and select your compute engine. 
4. In the drop down for license, select existing license if available or select ‘Issue a new license’. You can also provide a nick-name for your license.  
5. Click on Save Changes to register device and generate a license key.
6. Copy the generated license key and enter it in FlytConsole to complete the activation process of your device. The Activate Now tag at bottom right corner of FlytConsole should now turn green.

Hardware Setup
--------------

Visit :ref:`this link <hardware_setup_aero>` for details regarding hardware setup.

.. |br| raw:: html

   <br />