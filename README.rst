uritemplate
===========

Documentation_ -- GitHub_ -- BitBucket_ -- Travis-CI_

Simple python library to deal with `URI Templates`_. The API looks like

.. code-block:: python

    from uritemplate import URITemplate, expand

    # NOTE: URI params must be strings not integers

    gist_uri = 'https://api.github.com/users/sigmavirus24/gists{/gist_id}'
    t = URITemplate(gist_uri)
    print(t.expand(gist_id='123456'))
    # => https://api.github.com/users/sigmavirus24/gists/123456

    # or
    print(expand(gist_uri, gist_id='123456'))

    # also
    t.expand({'gist_id': '123456'})
    print(expand(gist_uri, {'gist_id': '123456'}))

Where it might be useful to have a class

.. code-block:: python

    import requests

    class GitHubUser(object):
        url = URITemplate('https://api.github.com/user{/login}')
        def __init__(self, name):
            self.api_url = url.expand(login=name)
            response = requests.get(self.api_url)
            if response.status_code == 200:
                self.__dict__.update(response.json())

When the module containing this class is loaded, ``GitHubUser.url`` is 
evaluated and so the template is created once. It's often hard to notice in 
Python, but object creation can consume a great deal of time and so can the 
``re`` module which uritemplate relies on. Constructing the object once should 
reduce the amount of time your code takes to run.

Installing
----------

::

    pip install uritemplate

License
-------

Modified BSD license_


.. _Documentation: http://uritemplate.rtfd.org/
.. _GitHub: https://github.com/sigmavirus24/uritemplate
.. _BitBucket: https://bitbucket.org/icordasc/uritemplate
.. _Travis-CI: https://travis-ci.org/sigmavirus24/uritemplate
.. _URI Templates: http://tools.ietf.org/html/rfc6570
.. _license: https://github.com/sigmavirus24/uritemplate/blob/master/LICENSE
