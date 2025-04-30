The package `ros-apt-source <https://github.com/ros-infrastructure/ros-apt-source/>`_ provides keys and source configuration for the ROS repositories.

You will need install this package con configure ROS repositories.

.. code-block:: console

   $ curl -O --output-dir /tmp/ https://ftp.osuosl.org/pub/ros/packages.ros.org/ros2-testing/ubuntu/pool/main/r/ros-apt-source/ros2-apt-source_1.0.0~$(. /etc/os-release && echo $VERSION_CODENAME)_all.deb
   $ sudo apt install /tmp/ros2-apt-source_1.0.0~$(. /etc/os-release && echo $VERSION_CODENAME)_all.deb
