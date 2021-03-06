.. _edison_guide:


FlytOS Installation guide for Intel Edison
==========================================

..  FlytOS requires a lot of dependencies to be installed. For this, we have provided the following two approaches:

.. * :ref:`Flashing FlytOS Linux Image <FlytOS_linux_image>` 
.. * :ref:`Installing FlytOS dependencies in your custom image<install_dependencies>`

.. Preparing your Intel Edison Board
.. ----------------------------------

.. _FlytOS_linux_image_edison:

Flashing FlytOS Linux Image
^^^^^^^^^^^^^^^^^^^^^^^^^^^

This section will help you in flashing FlytOS Linux Image on your Intel Edison Board.
This step requires you to have a registered FlytBase account. In case you don't have an account, :ref:`create a FlytBase Account<create_flytbase_account>` before you proceed.

**Image download:**

1. `Login <http://my.flytbase.com>`_ to your FlytBase Account.
2. Download the hardware specific `FlytOS Linux Image <http://my.flytbase.com/FlytOS>`_ from your FlytBase account.
3. Download size of the image is about 1.5 GBs.
4. Check *MD5 Hash* to verify the integrity of downloaded file:

   * Linux- launch a terminal and execute the following command ``md5sum <path-to-downloaded-image>/flyt*.img.gz``
   * Windows- launch a command window and execute the following command ``CertUtil -hashfile <path-to-downloaded-image>/flyt*.img.gz MD5``
   * Mac OS- launch a terminal and execute the following command ``md5 <path-to-downloaded-image>/flyt*.img.gz``
5. Compare the MD5 Hash generated to *MD5 Hash* mentioned in the `download page <http://my.flytbase.com/FlytOS>`_.

.. 6. Uncompress/extract the downloaded image:

..    * Linux- launch a terminal and execute the following command ``gunzip <path-to-downloaded-image>/flyt*.img.gz``.
..    * Windows- download and install 7-zip from `here <http://www.7-zip.org/download.html>`_. Extract downloaded image using 7-zip.
..    * Mac OS- launch a terminal and execute the following command ``gunzip <path-to-downloaded-image>/flyt*.img.gz``.
.. 7. Uncompressed size of image is about 4GBs.
      
**Image flash:**

.. 1. We recommend using a 32 GB SD Card, but a 16 GB card would work fine too. 
.. 2. Format the micro SD Card.
.. 3. Follow `this <http://odroid.com/dokuwiki/doku.php?id=en:odroid_flashing_tools>`_ guide to install the image on ODROID-XU4’s SD/eMMC card.


.. **Expanding SD Card partition:**

.. Since the image is only around 8.5 GBs, the rest of the SD Card would have unallocated memory. Follow `this guide <http://elinux.org/RPi_Resize_Flash_Partitions>`_ to expand the partition to the maximum possible size to utilize all memory.

We have created a video tutorial demonstrating how to flash FlytOS Linux Image on your Intel Edison board. Additionally one can refer Step-2 of `this guide <https://github.com/oskarpearson/mmeowlink/wiki/Backing-up-and-cloning-your-OpenAPS-Edison#step-2-flash-image-onto-edison>`_ for the same.

|br|

..  youtube:: LHMF8peY2MY
    :aspect: 16:9
    :width: 80%


|br|

**User Credentials**

All FlytOS Linux Image versions have the same Login user credentials: **username - flytpod & password - flytpod**

.. note:: Intel Edison will boot up with its wifi configured as Access Point. For more details, check :ref:`here<edison_wifiap>`.


.. _installing_flytos_edison:


Installing FlytOS debian package
--------------------------------

.. note:: This step requires you to have a registered FlytBase Account. In case you don't have an account, :ref:`create a FlytBase Account<create_flytbase_account>` before you proceed. 

Once you have installed the latest FlytOS Linux Image, you **MUST update your FlytOS debian package** by following the steps below:

1. `Login <http://my.flytbase.com>`_ to your FlytBase Account.
2. Download the hardware specific `FlytOS Debian Package <http://my.flytbase.com/FlytOS>`_ from your FlytBase Account.
3. Install some dependencies - To install run the following commands in your terminal.

   .. literalinclude:: include/flytos_dependency.sh
      :language: bash	

4. Once you have downloaded the Debian package, run the following command in your terminal to install FlytOS: 
   
.. code-block:: bash
   
   #For Intel Edison
   $ sudo dpkg -i <path to debian package location>/flytos_*.deb 

5. Check for **Congratulations! FlytOS installation completed** message at the end.
6. Just in case you see any dependency issues cropping up in your screen while installing FlytOS, kindly run the following command and execute the previous command again:
   
.. code-block:: bash
   
   $ sudo apt -f -y install

.. caution:: You must :ref:`activate your device<activate_flytos_edison>`, without which critical APIs would not function.


.. _flytos_basics_edison:


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


.. _activate_flytos_edison:

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

Visit :ref:`this link <hardware_setup_edison>` for details regarding hardware setup.

.. |br| raw:: html

   <br />