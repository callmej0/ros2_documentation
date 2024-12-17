Reading from a bag file (C++)
=============================

**Goal:** Learn how to use ``ament_lint`` and related tools to identify and fix code quality issues

**Tutorial level:** Advanced

**Time:** 10 minutes

.. contents:: Contents
   :depth: 2
   :local:

Background
----------

The ``ament`` family of CLI tools are used for CMake-based software development with ROS 2.
Ament ships with a collection of CLI programs that can help users write code that meet the ROS 2 coding standards.
Using these tools can greatly increase development velocity and help users write ROS applications and core code that meet the doc:`the ROS project's coding standards. <../../The-ROS2-Project/Contributing/Code-Style-Language-Versions>`.
We recommend that ROS developers familiarize themselves with these tools and use them before submitting their final pull requests. 

Prerequisites
-------------

You should have the ``ament`` packages installed as part of your regular ROS 2 setup.

If you need to install ROS 2, see the :doc:`Installation instructions <../../Installation>`.

You should have already completed the :doc:`basic ROS 2 bag tutorial <../Beginner-CLI-Tools/Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data>`, and we will be using the ``subset`` bag you created there.

Ament Lint CLI Tools
--------------------

1 ``ament_copyright``
^^^^^^^^^^^^^^^^^^^^^

The ``ament_copyright`` CLI can be used to check and update the copyright declaration in ROS source code.
This tool can also be used to check for the presence of an appropriate software license, copyright year, and copyright holders in your source code.
The ``ament_copyright`` tool works relative to the directory in which it is called, and walks the subdirectories and checks each source file within the directory [And dependencies?].
You can use``ament_copyright` to check your ROS package, ROS workspace, directory, or a single source file by simply moving to the appropriate root directory and calling the command.
``ament_copyright`` can also be used to used to automatically apply a copyright and license to source code files that are missing them. 


1.1 ``ament_copyright`` Arguments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default ``ament_copyright`` walks the directory in which it is called, including subdirectories and returns a report that lists all files that are missing a copyright notice.
The program takes a single optional argument which is a list of directories that should be scanned for the report.
For example, if you wish to scan just source and header files for copyright notices you can call `` ament_copyright ./src ./include``.

1.2 ``ament_copyright`` Options
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``ament_copyright`` supports the following options:

* ``-h, --help`` - shows a help message and exit
* ``--exclude [filename ...]`` - The filenames to exclude.
* ``--add-missing COPYRIGHT_NAME LICENSE`` - Add missing copyright notice and license information using the passed copyright holder and license. ``LICENSE`` passed to this option is the name of the license to be used. A full list of available licenses can be found by calling ``ament_copyright --list-licenses``
* ``--add-copyright-year`` - Add the current year to existing copyright notices.
* ``--list-copyright-names`` - List names of known copyright holders.
* ``--list-licenses`` - List names of known licenses )
* ``--verbose`` - Show all files instead of only the ones with errors / modifications (default: False)
* ``--xunit-file XUNIT_FILE`` - Generate a xunit compliant XML file (default: None)

1.3 ``ament_copyright`` Example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To check if your ROS package has an appropriate copyright and license file simply call ``ament_copyright`` with no arguments.
Using the ``--verbose`` option will list all checked files.

.. code-block:: console

  ament_copyright --verbose

If you happen to find an issue with one of your files you can address it by calling the following command.

2 ``ament_cppcheck``
^^^^^^^^^^^^^^^^^^^^^

The ``ament_cppcheck`` command line tool can be used to quickly perform static analysis of C++ source code files.
`Static analysis <https://en.wikipedia.org/wiki/Static_program_analysis>`_ is the process of automatically reviewing source code files for patterns that can often cause issues after compilation.
Some versions of cpp_check, the underlying utility used by ``ament_cppcheck``, can be rather slow.
For this reason ``ament_cpp_check`` may be disabled on some systems.
To enable it, you simply need to set the ``AMENT_CPPCHECK_ALLOW_SLOW_VERSIONS`` environment variable. 


2.1 ``ament_cppcheck`` Arguments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default ``ament_cppcheck`` walks the directory in which it is called, including subdirectories and returns a report that lists all of the potential issues in a source code file.
The program takes a single optional argument which is a list of directories that should be scanned for the report.
For example, if you wish to scan just a recently modified file you can call ``ament_cppcheck ./src/my_cpp_file.cpp``.

2.2 ``ament_cppcheck`` Options
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``ament_cppcheck`` supports the following options:

* ``-h, --help``- Show a help message and exit.
* ``--libraries [LIBRARIES ...]`` - Library configurations to load in addition to the standard libraries of C and C++. Each library is passed to cppcheck as '--library=<library_name>'
* ``--include_dirs [INCLUDE_DIRS ...]`` - Include directories for C/C++ files being checked.Each directory is passed to cppcheck as '-I <include_dir>' (default: None)
* ``--exclude [EXCLUDE ...]`` - Exclude C/C++ files from being checked.
* ``--xunit-file XUNIT_FILE`` - Generate a xunit compliant XML file (default: None)
* ``--cppcheck-version`` - Get the cppcheck version, print it, and then exit.

2.3 ``ament_cppcheck`` Example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create the following simple C++ program in a file named ``example.cpp``.

.. code-block:: cpp

  int main()
  {
      char a[10];
      a[10] = 0;
      return 0;
  }


This simple program accesses a part of memory out of bounds of the allocated array.Running
 Running ``ament_cppcheck`` in the directory with the file will yield the following results:

.. code-block:: console

   > ament_cppcheck
   [example.cpp:4]: (error: arrayIndexOutOfBounds) Array 'a[10]' accessed at index 10, which is out of bounds.
   >


