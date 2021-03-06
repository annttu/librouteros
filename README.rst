.. image:: https://travis-ci.org/luqasz/librouteros.svg?branch=master
    :target: https://travis-ci.org/luqasz/librouteros
    :alt: Tests

.. image:: https://img.shields.io/pypi/v/librouteros.svg
    :target: https://pypi.python.org/pypi/librouteros/
    :alt: Latest PyPI version

.. image:: https://img.shields.io/pypi/pyversions/librouteros.svg
    :target: https://pypi.python.org/pypi/librouteros/
    :alt: Supported Python Versions

.. image:: https://img.shields.io/pypi/l/librouteros.svg
    :target: https://pypi.python.org/pypi/librouteros/
    :alt: License

About
=====
Python implementation of `routeros api <http://wiki.mikrotik.com/wiki/API>`_. This library uses `semantic versioning <http://semver.org/>`_. On major version things may break, so pin version in dependencies.

Usage
=====
Before using this library, you need to create and connect to a socket. If using plain TCP socket, easiest way is to use `socket.create_connection() <https://docs.python.org/library/socket.html#socket.create_connection>`_.
For SSL/TLS I'd recommend using ``ssl.warp_socket(connected_socket, ciphers='ALL')`` as a bare minimum. This will work even when no certificate is set on remote device.

.. code-block:: python

    In [1]: from librouteros import login

    In [2]: api = login(username='admin', password='abc', sock=connected_socket)

    In [4]: api(cmd='/interface/print', stats=True)
    Out[4]:
      ({'.id': '*1',
      'bytes': '418152/157562',
      'comment': '',
      'disabled': False,
      'drops': '0/0',
      'dynamic': False,
      'errors': '0/0',
      'mtu': 1500,
      'name': 'ether1',
      'packets': '3081/1479',
      'running': True,
      'type': 'ether'},)

    In [5]: api(cmd='/interface/print')
    Out[5]:
      ({'.id': '*1',
      'comment': '',
      'disabled': False,
      'dynamic': False,
      'mtu': 1500,
      'name': 'ether1',
      'running': True,
      'type': 'ether'},)

    In [7]: api.close()

    In [8]:


If you want to pass parameters that start with a dot character you can do it in this way:

.. code-block:: python

    params = {'disabled': True, '.id' :'*7'}
    api(cmd='/ip/firewall/nat/set', **params)

Note that ``.id`` must always be passed as read from API. They usually start with a ``*`` followed by a number.
Keep in mind that they do change across reboots. As a rule of thumb, always read them first.


Python booleans are converted according to this table:

====== ========= ========
python direction api
====== ========= ========
False  <-        false,no
True   <-        true,yes
False  ->        no
True   ->        yes
====== ========= ========


Contributing
============
To submit a feature requests or a bug report, please use issues from within github. If you would like to submit a patch please contact author or use pull request.
