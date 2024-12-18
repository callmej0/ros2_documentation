Ament Lint CLI Utilities
========================

**Goal:** Learn how to use ``ament_lint`` and related tools to identify and fix code quality issues.

**Tutorial level:** Advanced

**Time:** 10 minutes

.. contents:: Contents
   :depth: 2
   :local:

Background
----------

The ``ament`` family of CLI tools are used for CMake-based software development with ROS 2.
Ament ships with a collection of CLI programs that can help users write code that meet the ROS 2 coding standards.
Using these tools can greatly increase development velocity and help users write ROS applications and core code that meet the doc:`the ROS project's coding standards <../../The-ROS2-Project/Contributing/Code-Style-Language-Versions>`.
We recommend that ROS developers familiarize themselves with these tools and use them before submitting their final pull requests. 

Prerequisites
-------------

You should have the ``ament`` packages installed as part of your regular ROS 2 setup.

If you need to install ROS 2, see the :doc:`Installation instructions <../../Installation>`.


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
^^^^^^^^^^^^^^^^^^^^

The ``ament_cppcheck`` command line tool can be used to quickly perform static analysis of C++ source code files.
`Static analysis <https://en.wikipedia.org/wiki/Static_program_analysis>`_ is the process of automatically reviewing source code files for patterns that can often cause issues after compilation.
Some versions of cpp_check, the underlying utility used by ``ament_cppcheck``, can be rather slow.
For this reason ``ament_cppcheck`` may be disabled on some systems.
To enable it, you simply need to set the ``AMENT_CPPCHECK_ALLOW_SLOW_VERSIONS`` environment variable. 


2.1 ``ament_cppcheck`` Arguments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default ``ament_cppcheck`` walks the directory in which it is called, including subdirectories and returns a report that lists all of the potential issues in a source code file.
The program takes a single optional argument which is a list of directories that should be scanned for the report.
For example, if you wish to scan just a recently modified file you can call ``ament_cppcheck ./src/my_cpp_file.cpp``.

2.2 ``ament_cppcheck`` Options
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``ament_cppcheck`` supports the following options:

* ``-h, --help``- Show a help message and exit.
* ``--libraries [LIBRARIES ...]`` - Library configurations to load in addition to the standard libraries of C and C++. Each library is passed to cppcheck as '--library=<library_name>'
* ``--include_dirs [INCLUDE_DIRS ...]`` - Include directories for C/C++ files being checked.Each directory is passed to cppcheck as '-I <include_dir>' (default: None)
* ``--exclude [EXCLUDE ...]`` - Exclude C/C++ files from being checked.
* ``--xunit-file XUNIT_FILE`` - Generate a xunit compliant XML file (default: None)
* ``--cppcheck-version`` - Get the cppcheck version, print it, and then exit.

2.3 ``ament_cppcheck`` Example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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


3 ``ament_cpplint``
^^^^^^^^^^^^^^^^^^^

``ament_cpplint`` can be used to check your C++ code against the `Google style conventions <https://google.github.io/styleguide/cppguide.html>`_ using `cpplint <https://github.com/cpplint/cpplint?tab=readme-ov-file>`_.
``ament_cpplint`` will scan the current directory and subdirectories for all C++ header and source files and apply the CppLint application to the file and return the results.
At this time ``ament_cpplint`` is unable to address issues it finds automatically, if you would like to fix the formatting issues automatically please see ``ament_uncrustify``.


3.1 ``ament_cpplint`` Arguments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The program takes a single optional argument which is a list of directories that should be scanned for the report.
For example, if you wish to scan just source and header files for copyright notices you can call `` ament_copyright ./src ./include``.


3.2 ``ament_cpplint`` Options
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* ``-h, --help`` - Show a help message and exit.
* ``--filters FILTER,FILTER,...`` - A comma separated list of category filters to apply.
* ``--linelength N`` - The maximum line length (default: 100).
* ``--root ROOT`` - The --root option for cpplint.


3.3 ``ament_cpplint`` Example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Let's create a simple C++ program named ``example.cpp``.
We will add a few lines of code that violate coding standards


.. code-block:: cpp

  int main()
  {
       int a = 10;
       int b = 10;
       int c = 0;  
       if( a == b)  {
 	  c=a;}  
       return 0;
   }


Applying ``ament_cpplint`` to this file will yield the following errors:

.. code-block:: console

  example.cpp:0:  No copyright message found.  You should have a line: "Copyright [year] <Copyright Owner>"  [legal/copyright] [5]
  example.cpp:5:  Line ends in whitespace.  Consider deleting these extra spaces.  [whitespace/end_of_line] [4]
  example.cpp:7:  Tab found; better to use spaces  [whitespace/tab] [1]
  example.cpp:7:  Line ends in whitespace.  Consider deleting these extra spaces.  [whitespace/end_of_line] [4]
  example.cpp:7:  Missing spaces around =  [whitespace/operators] [4]


4 ``ament_flake8``
^^^^^^^^^^^^^^^^^^

`Flake8 <https://pypi.org/project/flake8/>`_ is a Python tool for linting and style enforcement.
The ``ament_flake8`` command line tool can be used to quickly perform linting of Python source code files using `Flake8 <https://pypi.org/project/flake8/>`_.
This tool will help you locate and fix minor errors and style problems with your ROS Python programs such as trailing whitespace, overly long lines of code, poorly spaced function arguments, and much more!
Unfortunately, at this time, ``ament_flake8`` is unable to to automatically fix these issues.
Deveopers who whould like to address linting issues can use `Python's Black utility <https://github.com/psf/black>`_ to fix formatting issues automatically. 

4.1 ``ament_flake8`` Arguments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The program takes a single optional argument which is a list of directories that should be scanned for the report.
For example, if you wish to scan just one package in your workspace you can call ``ament_flake8`` directly in the package's working directory or pass it a path to the directory.


4.2 ``ament_flake8`` Options
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* ``-h, --help`` - show a help message and exit
* ``--config path`` - The config file used (default: /opt/ros/humble/lib/python3.10/site-packages/ament_flake8/configuration/ament_flake8.ini)
* ``--linelength N`` - The maximum line length (default specified in the config file)
* ``--exclude [filename ...]`` - the filenames to exclude. (default: None)
* ``--xunit-file XUNIT_FILE`` - generate a xunit compliant XML file (default: None)

4.3 ``ament_flake8`` Example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create the following simple Python program in a file named ``example.py``.

.. code-block:: python

  def uglyPythonFunction(a,b,  c):
      if a != b:
          print("A does not match b")
      thisIsAVariableNameThatIsWayTooLongLongLong = 2
      extra_long =(thisIsAVariableNameThatIsWayTooLongLongLong*thisIsAVariableNameThatIsWayTooLongLongLong )
      return(c)

Applying ``ament_flake8`` to this file will result in the following errors.

.. code-block:: console

  example.py:1:25: E231 missing whitespace after ','
  def uglyPythonFunction(a,b,  c):
  
  
  example.py:5:5: F841 local variable 'extra_long' is assigned to but never used
      extra_long =(thisIsAVariableNameThatIsWayTooLongLongLong*thisIsAVariableNameThatIsWayTooLongLongLong )
      ^
  
  example.py:5:17: E225 missing whitespace around operator
      extra_long =(thisIsAVariableNameThatIsWayTooLongLongLong*thisIsAVariableNameThatIsWayTooLongLongLong )
                  ^
  
  example.py:5:100: E501 line too long (106 > 99 characters)
      extra_long =(thisIsAVariableNameThatIsWayTooLongLongLong*thisIsAVariableNameThatIsWayTooLongLongLong )
                                                                                                     ^
  
  example.py:5:105: E202 whitespace before ')'
      extra_long =(thisIsAVariableNameThatIsWayTooLongLongLong*thisIsAVariableNameThatIsWayTooLongLongLong )
                                                                                                          ^
  
  1     E202 whitespace before ')'
  1     E225 missing whitespace around operator
  1     E231 missing whitespace after ','
  1     E501 line too long (106 > 99 characters)
  1     F841 local variable 'extra_long' is assigned to but never used

  1 files checked
  5 errors

  'E'-type errors: 4
  'F'-type errors: 1

  Checked files:

  * example.py


If you have installed Python's `Black utility <https://github.com/psf/black>`_ it is possible to address these issues directly by calling ``black example.py.``

5 ``ament_uncrustify``
^^^^^^^^^^^^^^^^^^^^^^

`Uncrustify <https://github.com/uncrustify/uncrustify>`_ is a C++ linting tool, similar to ``ament_cpplint``, that has the advantage that it can **automatically fix** the issues it finds!
This tool will help you locate and fix minor errors and style problems with your C++ ROS programs such as trailing whitespace, overly long lines of code, poorly spaced function arguments, and much more!


5.1 ``ament_uncrustify`` Arguments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The program takes a single optional argument which is a list of directories that should be scanned for the report.
For example, if you wish to scan just one package in your workspace you can call ``ament_uncrustify`` directly in the package's working directory or pass it a path to the directory.


5.2 ``ament_uncrustify`` Options
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* ``-h, --help`` - show a help message and exit.
* ``-c CFG`` - The config file.
* ``--linelength N`` - The maximum line length.
* ``--language`` - One of {C,C++,CPP}, passed to uncrustify as '-l <language>' to force a specific language rather then choosing one based on file extension.
* ``--reformat`` -  Reformat the files in place, i.e. fix the formatting errors encountered.
* ``--exclude [filename ...]`` - the filenames to exclude. (default: None)
* ``--xunit-file XUNIT_FILE`` - generate a xunit compliant XML file (default: None)

5.3 ``ament_uncrustify`` Example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Let's return to the simple C++ program named ``example.cpp``.

.. code-block:: cpp

  int main()
  {
       int a = 10;
       int b = 10;
       int c = 0;  
       if( a == b)  {
 	  c=a;}  
       return 0;
   }


Applying ``ament_uncrustify example.cpp`` to this file will yield the following output.

.. code-block:: cpp

  --- example.cpp
  +++ example.cpp.uncrustify
  @@ -1,9 +1,10 @@
  -  int main()
  -  {
  -       int a = 10;
  -       int b = 10;
  -       int c = 0;  
  -       if( a == b)  {
  - 	  c=a;}  
  -       return 0;
  -   }
  +int main()
  +{
  +  int a = 10;
  +  int b = 10;
  +  int c = 0;
  +  if (a == b) {
  +    c = a;
  +  }
  +  return 0;
  +}
  1 files with code style divergence

To apply these changes to the file we can run ``ament_uncrustify`` with the ``--reformat`` flag.
**With this option specified uncrustify will apply the necessary changes in place, saving us a lot of time, especially when working with a larger codebase!**
