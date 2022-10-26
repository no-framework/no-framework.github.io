+++
title = "Instance Provider"
weight = 10
+++


The instance provider is here to help you keep code decoupled so you can re-use
it in ways you never expected.

If you used an IoC container this will feel familiar to you, the only
difference is it is done without any third-party library.

```python
from some_module.internals import UsefulClass, Details

class SomeModuleProvider:
    def provide_instance_for_other_modules(self)
        """
	By naming convention, "instance_for_other_modules" is the "name" of
	these instances, they not accept any parameters to modify values
        because they become "unnamed" if that happens, (you don't know what it will
        return). That can be done with _create methods.
        """
        details_for_useful_class = self._provide_instance_privately_to_this_provider()
        return UsefulClass(details_for_useful_class)

    def _provide_instance_privately_to_this_provider(self)
        """
	Same as above, except you don't want anyone to use these externally.
        They're private.
        """
        return Details("Private")

    def _create_code_duplicated_often(self, modifier1, modifier2)
        """
	Useful when you want to avoid duplcating code freqnetly to change some
	details. Concrete instances still need to go through a provider
        however. Public crete methods are not allowed.
        """
        return Details(modifier1 + modifier2)
```

If you wish to return a single-instance for a method that is called many times,
use of the `functools.cache` decorator is an easy way to get a "singleton" on
the instance of a provider without enforcing it on every software component
that uses this provider.


The above code would look like this if we were using `python-dependency-injector` instead:

```python
from dependency_injector import containers, providers
from some_module.internals import UsefulClass, Details

def _create_code_duplicated_often(self, modifier1, modifier2)
    return Details(modifier1 + modifier2)

class Container(containers.DeclarativeContainer):
    _detail_provider = providers.Factory(Details)
    useful_class_provider = providers.Object(UsefulClass, _detail_provider)
```

We don't permit logic inside providers, they are strictly there to orchestrate
how "infrastructure" is put together. Detecting if "the environment is dev" is
also not permitted anywhere and generally a bad practice.

Using a provider is only permitted in other providers or in `run.py` file.

The provider structure is intentionally designed to provide a new instance by
default instead of re-use instances (re-use is instead an opt-in behaviour).
This means when you have two or three run.py files in a project you can
deterministically map out where side-effects come from, but also you introduce
"bulkheads" against side-effects from other instances. Breaking the database
connection used by one component doesn't need to break the database connections
for everything else in your application unless you specifically want it to.

This also has advantages for threaded applications as you now have fine-grained
control over what instances are shared and which ones are re-created in
threads.

The downside is that the overall memory footprint might be a bit larger.
Because we're mostly talking about "infrastructure" you should expect the
memory footprint to not grow with data processed through the application but
rather how often a provider is used by another module. If you have a common
component used 10 times it will create 10 instances. This is manageable however
as the memory footprint of your infrastructure will be deterministic and
measurable the moment you "run" your application, you do not need to test it
with different workloads, everything is provisioned in memory once the
application starts. This also means the difference between memory footprint at
any point at runtime and the size when the application started is equal to the
amount of "data state" being held in the system.

