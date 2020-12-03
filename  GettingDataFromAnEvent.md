# This page describes how to access data from a triggered or simulated physis event

#introduction
edm::Event class provides access to all data(raw data, HLT, reconstructed, analysis, etc.) associated with a triggered or simulated event

#Identifying data in an event
EDM provides several ways of Identifying data in an event: labels, ProductID, and provenance

#Labels There are three string 'labels' that are used to uniquely identify a datum of a certain C++ type
##Module Labels
The module label is the persistent label assigned in the configuration file used by the job that actually created the datum.
process.foo = cms.EDproducer("MyFooProducer")
foo is the module label. all objects created by that module will be assigned that labels
If the datum was first created by an input source, the string "source" is the module label
process.source = cms.Source("PythisSource")
input sources such as PoolSource that just read pre-existing events do not create or modify any module labels, ProductID's or any provenance
##product instance label
The product instance label (persistent) is assigned to a product by the producer or source (either set at compile time or via a parameter). This is used in the producer or source when registering the product (using produces<> method) and in the Event::put() method. Using Event::emplace() in conjunction with \edm::EDPutTokenT\ returned by the produces<> allows to specify the product instance label only once. A default value (which is the empty string) is allowed. When one module produces more than one product of the same type, each must be given a different product instance label.
##process name
the process name is the name used in the process statement in the configuration file for the
