# This page describes how to access data from a triggered or simulated physis event

## introduction
`edm::Event` class provides access to all data(raw data, HLT, reconstructed, analysis, etc.) associated with a triggered or simulated event

## Identifying data in an event
EDM provides several ways of Identifying data in an event: labels, ProductID, and provenance

## Labels There are three string 'labels' that are used to uniquely identify a datum of a certain C++ type
### Module Labels
The module label is the persistent label assigned in the configuration file used by the job that actually created the datum.
`process.foo = cms.EDproducer("MyFooProducer")`
foo is the module label. all objects created by that module will be assigned that labels
If the datum was first created by an input source, the string "source" is the module label
`process.source = cms.Source("PythisSource")`
input sources such as PoolSource that just read pre-existing events do not create or modify any module labels, ProductID's or any provenance
### product instance label
The product instance label (persistent) is assigned to a product by the producer or source (either set at compile time or via a parameter). This is used in the producer or source when registering the product (using produces<> method) and in the Event::put() method. Using `Event::emplace()` in conjunction with `edm::EDPutTokenT` returned by the `produces<>` allows to specify the product instance label only once. A default value (which is the empty string) is allowed. When one module produces more than one product of the same type, each must be given a different product instance label.
### process name
the process name is the name used in the process statement in the configuration file for the job that created the job
  process = cms.Process("RECO")
  ....
  }
Then 'RECO' would be the process name for all data items created by EDProducer's in this job.
### ProductID
Every top level product in an event is assigned a unique persistent integer value called the ProductID.
### EDGetToken
When registering what data is to be requested, the registration function returns a `edm::EDGetToken` or `edm::EDGetToken<T>` which can be used get the specific data
NOTE: It is possible to convert a `edm::EDGetToken<T>` to a `edm::EDGetToken` via a constructor call but it is not possible to go the other way.
### Provenance
Provenance keeps track of how a datum was produced.
* what type of EDProducer or InputSource first created the data
* the label assigned to the instance of the EDProducer in the job
* the parameters passed to the EDProducer or InputSource
* the product instance name associated with the datum
* what objects were gotten from the Event by the EDProducer
If you get a Handle to a datum in the Event, as described below, then you can access information about its provenance, such as the name of the process that produced it.
  edm::Handle<T> handel = evnet.getHandle(token);
  const Provenance& provenance = *handle.provenance();
  const string& processName = provenance.processName();

## Registering for data access
