DICOM Utilities
===============

Collection of libraries and applications that helps with DICOM testing and troubleshooting.

DCMTK - DICOM Toolkit
---------------------

DICOM tag (gggg,eeee) = group,element

.. code-block:: bash
	:caption: Delete DICOM tag

	dcmodify -e "(gggg, eeee)=value" filename

.. code-block:: bash
	:caption: Modify DICOM TAG

	dcmodify -m "(gggg, eeee)=value" filename

.. code-block:: bash
	:caption: All files in directory (delete/modify)

	find . -name "*.dcm" -exec dcmodify -e "(gggg, eeee)=value" \{\} \;
	find . -name "*.dcm" -exec dcmodify -m "(gggg, eeee)=value" \{\} \;

.. code-block:: bash
	:caption: Convert all files in directory to implicit little endian

	find . -name "*.dcm" -exec dcmconv +ti \{\}  \{\}.new \;
