
Spreadsheet
===========

Tips for preparing comma separated value (CSV) tabular files for import:

- Make sure that you use one date format per column (during mapping the date format string is specified)

- For integer coded cells make sure to use format of number general (1.00 is not the same as 1 when we are talking about codification)

- Make sure that all headers have values (if coulmn header is empty, there is no way for program to handle it)

Helper functions (LibreOffice - Calc):

.. code-block:: bash
	:caption: Split data to miltiple cells

	data->textToColumns


.. code-block:: bash
	:caption: Add text prefix to a data in cells

	="prefix"&A1

.. code-block:: bash
	:caption: Convert integer to text with 3 leading zeros

	=TEXT(A1, "000")

.. code-block:: bash
	:caption: Get rid of empty spaces in text

	=TRIM(A1)

.. code-block:: bash
	:caption: Get the integer part of decimal number

	=A1-MOD(A1, 1)
