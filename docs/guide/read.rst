Reading images
==============

There are several ways to open images:

- :ref:`To open an image file <open-image-file>`
- :ref:`To read a input stream (file-like object) that provides an image binary
  <read-input-stream>`
- :ref:`To read a binary string that contains image <read-blob>`
- :ref:`To copy an existing image object <clone-image>`

All of these operations are provided by the constructor of
:class:`~wand.image.Image` class.


.. _open-image-file:

Open an image file
------------------

The most frequently used way is just to open an image by its filename.
:class:`~wand.image.Image`'s constructor can take the parameter named
``filename``::

    from wand.image import Image

    with Image(filename='pikachu.png') as img:
        print 'width =', img.width
        print 'height =', img.height

.. note::

   It must be passed by keyword argument exactly. Because the constructor
   has many parameters that are exclusive to each other.

   There is a keyword argument named ``file`` as well, but don't confuse
   it with ``filename``. While ``filename`` takes a string of a filename,
   ``file`` takes a input stream (file-like object).


.. _read-input-stream:

Read a input stream
-------------------

If an image to open cannot be located by a filename but can be read through
input stream interface (e.g. opened by :func:`os.popen()`,
contained in :class:`~StringIO.StringIO`, read by :func:`urllib2.urlopen()`), 
it can be read by :class:`~wand.image.Image` constructor's ``file`` parameter.
It takes all file-like objects which implements :meth:`~file.read()` method::

    from urllib2 import urlopen
    from wand.image import Image

    response = urlopen('https://stylesha.re/minhee/29998/images/100x100')
    try:
        with Image(file=response) as img:
            print 'format =', img.format
            print 'size =', img.size
    finally:
        response.close()

In the above example code, ``response`` object returned by
:func:`~urllib2.urlopen()` function has :meth:`~file.read()` method,
so it also can be used as an input stream for a downloaded image.


.. _read-blob:

Read a blob
-----------

If you have just a binary string (:class:`str`) of the image, you can pass
it into :class:`~wand.image.Image` constructor's ``blob`` parameter to read::

    from wand.image import Image

    with open('pikachu.png') as f:
        image_binary = f.read()

    with Image(blob=image_binary) as img:
        print 'width =', img.width
        print 'height =', img.height

It is a way of the lowest level to read an image. There will probably not be
many cases to use it.


.. _clone-image:

Clone an image
--------------

If you have an image already and have to copy it for safe manipulation,
use :meth:`~wand.image.Image.clone()` method::

    from wand.image import Image

    with Image(filename='pikachu.png') as original:
        with original.clone() as converted:
            converted.format = 'png'
            # operations on a converted image...

For some operations like format converting or cropping, there are safe methods
that return a new image of manipulated result like
:meth:`~wand.image.Image.convert()` or slicing operator. So the above example
code can be replaced by::

    from wand.image import Image

    with Image(filename='pikachu.png') as original:
        with original.convert('png') as converted:
            # operations on a converted image...

