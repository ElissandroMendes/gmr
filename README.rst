===
gmr
===

.. image:: https://api.travis-ci.org/AlexanderFabisch/gmr.png?branch=master
   :target: https://travis-ci.org/AlexanderFabisch/gmr
   :alt: Travis

Gaussian Mixture Models (GMMs) for clustering and regression in Python.

Source code repository: https://github.com/AlexanderFabisch/gmr

.. image:: https://raw.githubusercontent.com/AlexanderFabisch/gmr/master/gmr.png

`(Source code of example) <https://github.com/AlexanderFabisch/gmr/blob/doc/save/examples/plot_regression.py>`_


Example
-------

Estimate GMM from samples and sample from GMM:

.. code-block:: python

    from gmr import GMM

    gmm = GMM(n_components=3, random_state=random_state)
    gmm.from_samples(X)
    X_sampled = gmm.sample(100)


For more details, see:

.. code-block:: python

    help(gmr)


Installation
------------

Install from `PyPI`_:

.. code-block:: bash

    sudo pip install gmr

or from source:

.. code-block:: bash

    sudo python setup.py install

.. _PyPi: https://pypi.python.org/pypi


How Does It Compare to scikit-learn?
------------------------------------

There is an implementation of Gaussian Mixture Models for clustering in
`scikit-learn <http://scikit-learn.org/stable/modules/generated/sklearn.mixture.GMM.html>`_
as well. Regression could not be easily integrated in the interface of
sklearn. That is the reason why I put the code in a separate repository.
It is possible to initialize GMR from sklearn though:

.. code-block:: python

    from sklearn.mixture import GaussianMixture
    from gmr import GMM
    gmm_sklearn = GaussianMixture(n_components=3, covariance_type="diag")
    gmm_sklearn.fit(X)
    gmm = GMM(
        n_components=3, priors=gmm_sklearn.weights_, means=gmm_sklearn.means_,
        covariances=np.array([np.diag(c) for c in gmm_sklearn.covariances_]))


Gallery
-------

.. image:: https://raw.githubusercontent.com/AlexanderFabisch/gmr/master/doc/sklearn_initialization.png
    :width: 60%

`Diagonal covariances <https://github.com/AlexanderFabisch/gmr/blob/master/examples/plot_iris_from_sklearn.py>`_

.. image:: https://raw.githubusercontent.com/AlexanderFabisch/gmr/master/doc/confidence_sampling.png
    :width: 60%

`Sample from confidence interval <https://github.com/AlexanderFabisch/gmr/blob/master/examples/plot_sample_mvn_confidence_interval.py>`_

.. image:: https://raw.githubusercontent.com/AlexanderFabisch/gmr/master/doc/trajectories.png
    :width: 60%

`Generate trajectories <https://github.com/AlexanderFabisch/gmr/blob/master/examples/plot_trajectories.py>`_

.. image:: https://raw.githubusercontent.com/AlexanderFabisch/gmr/master/doc/time_invariant_trajectories.png
    :width: 60%

`Sample time-invariant trajectories <https://github.com/AlexanderFabisch/gmr/blob/master/examples/plot_time_invariant_trajectories.py>`_

You can find all examples `here <https://github.com/AlexanderFabisch/gmr/tree/master/examples>`_.


Saving a Model
--------------

This library does not directly offer a function to store fitted models. Since
the implementation is pure Python, it is possible, however, to use standard
Python tools to store Python objects. For example, you can use pickle to
temporarily store a GMM:

.. code-block:: python

    import numpy as np
    import pickle
    import gmr
    gmm = gmr.GMM(n_components=2)
    gmm.from_samples(X=np.random.randn(1000, 3))

    # Save object gmm to file 'file'
    pickle.dump(gmm, open("file", "wb"))
    # Load object from file 'file'
    gmm2 = pickle.load(open("file", "rb"))

It might be required to store models more permanently than in a pickle file,
which might break with a change of the library or with the Python version.
In this case you can choose a storage format that you like and store the
attributes `gmm.priors`, `gmm.means`, and `gmm.covariances`. These can be
used in the constructor of the GMM class to recreate the object and they can
also be used in other libraries that provide a GMM implementation. The
MVN class only needs the attributes `mean` and `covariance` to define the
model.


Original Publication(s)
-----------------------

The first publication that presents the GMR algorithm is

    [1] Z. Ghahramani, M. I. Jordan, "Supervised learning from incomplete data via an EM approach," Advances in Neural Information Processing Systems 6, 1994, pp. 120-127, http://papers.nips.cc/paper/767-supervised-learning-from-incomplete-data-via-an-em-approach

but it does not use the term Gaussian Mixture Regression, which to my knowledge occurs first in

    [2] S. Calinon, F. Guenter and A. Billard, "On Learning, Representing, and Generalizing a Task in a Humanoid Robot," in IEEE Transactions on Systems, Man, and Cybernetics, Part B (Cybernetics), vol. 37, no. 2, 2007, pp. 286-298, doi: `10.1109/TSMCB.2006.886952 <https://doi.org/10.1109/TSMCB.2006.886952>`_.

A recent survey on various regression models including GMR is the following:

    [3] F. Stulp, O. Sigaud, "Many regression algorithms, one unified model: A review," in Neural Networks, vol. 69, 2015, pp. 60-79, doi: `10.1016/j.neunet.2015.05.005 <https://doi.org/10.1016/j.neunet.2015.05.005>`_.
