===================================
Ubidots Ruby API Client
===================================

The Ubidots Ruby API Client makes calls to the `Ubidots Api <http://things.ubidots.com/api>`_. 

Installation
------------

Add this line to your application's Gemfile:.

.. code-block:: ruby

    gem 'ubidots'

And then execute:

.. code-block:: bash

    $ bundle

Or install it yourself as:

.. code-block:: bash

    $ gem install ubidots


Connecting to the API
----------------------

Before playing with the API you must be able to connect to it using your private API key, which can be found `in your profile <http://app.ubidots.com/userdata/api/>`_.

If you don't have an account yet, you can `create one here <http://app.ubidots.com/accounts/signup/>`_.

Once you have your API key, you can connect to the API by creating an ApiClient instance. Let's assume your API key is: "7fj39fk3044045k89fbh34rsd9823jkfs8323". Then your code would look like this:


.. code-block:: ruby

    require 'ubidots'
    
    @api = Ubidots::ApiClient.new("7fj39fk3044045k89fbh34rsd9823jkfs8323")


Now you have an instance of ApiClient ("api") which can be used to connect to the API service.

Saving a new Value to a Variable
--------------------------------

Retrieve the variable you'd like the value to be saved to:

.. code-block:: ruby
    
    my_variable = @api.get_variable('56799cf1231b28459f976417')

Given the instantiated variable, you can save a new value with the following line:

.. code-block:: ruby
    
    new_value = my_variable.save_value( {'value'=>10} )

You can also specify a timestamp (optional):

.. code-block:: ruby

    new_value = my_variable.save_value( {'value'=>10, 'timestamp'=>1376061804407} )

If no timestamp is specified, the API server will assign the current time to it. We think it's always better for you to specify the timestamp so the record reflects the exact time the value was captured, not the time it arrived to our servers.

Creating a DataSource
----------------------

As you might know by now, a data source represents a device or a virtual source.

This line creates a new data source:

.. code-block:: ruby
    
    new_datasource = @api.create_datasource( {"name"=>"myNewDs", "tags"=>["firstDs", "new"], "description"=>"any des"} )

The 'name' key is required, but the 'tags' and 'description' keys are optional. This new data source can be used to track different variables, so let's create one.


Creating a Variable
--------------------

A variable is a time-series containing different values over time. Let's create one:


.. code-block:: ruby
    
    new_variable = new_datasource.create_variable( {"name"=>"myNewVar", "unit"=>"Nw"} )

The 'name' and 'unit' keys are required.

Getting Values
--------------

To get the values of a variable, use the method get_values in an instance of the class Variable. This will return a values array.

If you only want the last N values call the method with the number of elements you want.

.. code-block:: ruby
    
    all_values = my_variable.get_values()
    

Getting a group of Data sources
--------------------------------

If you want to get all your data sources you can a method on the ApiClient instance directly. This method return a objects Datasource array.

.. code-block:: ruby

    all_datasources = @api.get_datasources()


Getting a specific Data source
-------------------------------

Each data source is identified by an ID. A specific data source can be retrieved from the server using this ID.

For example, if a data source has the id 51c99cfdf91b28459f976414, it can be retrieved as follows:


.. code-block:: ruby

    my_specific_datasource = @api.get_datasource('51c99cfdf91b28459f976414')

Getting a group of  Variables from a Data source
-------------------------------------------------

With a data source. you can also retrieve some or all of its variables:

.. code-block:: ruby

    all_variables_of_datasource =  my_datasource.get_variables()


Getting a specific Variable
------------------------------

As with data sources, you can use your variable's ID to retrieve the details about it:

.. code-block:: ruby

    my_specific_variable = @api.get_variable('56799cf1231b28459f976417')