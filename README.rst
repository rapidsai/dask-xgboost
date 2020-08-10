Dask-XGBoost
============

Deprecation warning!
--------------------

*dask-xgboost is now deprecated*. As of release 1.0, XGBoost provides a
native Dask API:
https://xgboost.readthedocs.io/en/latest/tutorials/dask.html

The native XGBoost API has all of the functionality of dask-xgboost,
combined with an updated, clean API and much more extensive
testing. All users are encouraged to switch to the native API. See
also this blog post for an overview:
https://medium.com/rapids-ai/a-new-official-dask-api-for-xgboost-e8b10f3d1eb7


Overview
--------

Distributed training with XGBoost and Dask.distributed

This repository enables you to perform distributed training with XGBoost on
Dask.array and Dask.dataframe collections.

::

   pip install dask-xgboost

Example
-------

.. code-block:: python

   from dask.distributed import Client
   client = Client('scheduler-address:8786')  # connect to cluster

   import dask.dataframe as dd
   df = dd.read_csv('...')  # use dask.dataframe to load and
   df_train = ...           # preprocess data
   labels_train = ...

   import dask_xgboost as dxgb
   params = {'objective': 'binary:logistic', ...}  # use normal xgboost params
   bst = dxgb.train(client, params, df_train, labels_train)

   >>> bst  # Get back normal XGBoost result
   <xgboost.core.Booster at ... >

   predictions = dxgb.predict(client, bsg, data_test)


How this works
--------------

For more information on using Dask.dataframe for preprocessing see the
`Dask.dataframe documentation <http://dask.pydata.org/en/latest/dataframe.html>`_.

Once you have created suitable data and labels we are ready for distributed
training with XGBoost.  Every Dask worker sets up an XGBoost slave and gives
them enough information to find each other.  Then Dask workers hand their
in-memory Pandas dataframes to XGBoost (one Dask dataframe is just many Pandas
dataframes spread around the memory of many machines).  XGBoost handles
distributed training on its own without Dask interference.  XGBoost then hands
back a single ``xgboost.Booster`` result object.


Larger Example
--------------

For a more serious example see

-  `This blogpost <http://matthewrocklin.com/blog/work/2017/03/28/dask-xgboost>`_
-  `This notebook <https://gist.github.com/mrocklin/19c89d78e34437e061876a9872f4d2df>`_
-  `This screencast <https://youtu.be/Cc4E-PdDSro>`_

History
-------

Conversation during development happened at `dmlc/xgboost #2032
<https://github.com/dmlc/xgboost/issues/2032>`_
