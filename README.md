# hana-annotations
for C_HANADEV_15

https://training.sap.com/certification/c_hanadev_15-sap-certified-development-associate---sap-hana-20-sps03-g/

⋅⋅* Native SAP HANA applications > 12%
Setup a basic development project using XS Classic and XS Advanced.

⋅⋅* The Persistence Model > 12%
Develop a basic persistance model on SAP HANA for a nativa HANA application

⋅⋅* OData Services > 12%
Explain how to create OData services on SAP HANA and how to use them in SAPUI5

⋅⋅* SQLScript > 12%
Develop application logic using the SQLScript language

⋅⋅* SAP HANA Overview 8% - 12%
Know the architecture of the SAP HANA development platform

⋅⋅* Development Tools 8% - 12%
Use the right development tool for each development model of the SAP HANA platform.

⋅⋅* Building UIs with SAPUI5 8% - 12%
Develop a SAPUI5 application using Data Binding

⋅⋅* Server Side JavaScript 8% - 12%
Develop application logic using Server Side JavaScript


• OData Services > 12%

unit5 lesson 1 - Create OData services on SAPHANA XSA

Open Data Protocol is an OASIS standard that defies the best practice for building
and consuming RESTful APIs. http://www.odata.org/
Basics:
..*OData Data Model
...Provides a generic way to Organize/describe data wit Entity Data Model (EDM)
..*OData Protocol
...Enables client to query an OData service. 
...**REST-based create, read, update, and delete** + OData-defined query language
...sends data in either of ways: XML or JSON
..*Client Libraries
...Enables access to data via OData protocol
...Since most OData clients are applications, Pre-built libraries to request OData and display results
..*OData Services   
...**Exposes an end point** that allows access to data in the SAP HANA database

OData is a resource-based web protocol for querying & updating data
OData defines operations on resources using **HTTP commands (e.g GET,PUT,POST,DELETE)**
and specifies the uniform resource indicator (URI) syntax to use to identify the resources

**Data is transferred over HTTP using the Atom or JSON format**

**The main aim of OData is to define an abstract data model and a protocol, which,
combined, enable any client to access data exposed by any data source.**

The OData approac to data exchange involves the following elements:

..* OData Services os SAP HANA
To expose database info via OData in XSA, we have to use the Node,js module of the MTA app.

OData Capabilities in SAPHANA
..*Aggregation
...use functions to aggregate columns
..*Associations
...Express relationships between entities
..*Parameter Entity Sets
...input parameters for SAP HANA calculation views and analytic views
..*Key Specification
...**EntityType** denotes a set of properties forming a unique key.

**The scope of OData(JS) come along with XSJS/Node.js core app in the architecture.**

**OData Dev process**
..*Data End Point
...Define which data to expose (e.g table)
..*OData Service Definition
...Define mechanism to expose authorized data
..*UI Call to OData Service
...Link UI table view to the OData service
..*OData Display/Test
...URL filters: $top, $format, $orderby, $filter, $select...

**An OData service is defined in a file extension .xsodata**. It must contain at least the entry 
```javascript
service { "projName.dbNameFolder_db::Table.Column" as "nameThis"; }
```

which generates a OData service with an **empty service catalog and an empty metadata file.**
table becomes directly exposed and accessible via OData 
note: this with XSJS, v4 is different
note2: **Read-Write, by default, the created OData service is write-enable**, so a user
with roper authorizations on the destination table can use it to crud table lines
it is possible to forbid data mod via explicit options:
```javascript
service { "projName.dbNameFolder_db::Table"
            create forbidden
            update forbidden
            delete forbidden;
}
```

Custom Exits can be created to complet, or override, default crud service routines
..*Validation Exits
...used for validation of input data and data consistency checks. they can be executed
...before of after the change operation, or before of after the commit operations
..*Modification Exits
...we can define custom logic to crud an entry in an entity set. If a mod exit is
...specified, it is executed instead of the generic actions provided by the OData infrastructure.

unit5 lesson 2 - Describe the use of keys and associations in OData

OData Key Specification
The OData specifications requires an EntityType to denote a set properties forming a unique key.
**In SAPHANA, only tables can have a unique key, the primary key.**For all other(mostly views)
objects you need to specify a key for the entity.
```javascript
service { "sample.odata::view"
            as "MyView" key ("ID", "Text");
}
```

OData Association
when we define an association it requires that we specify a name, which ref two exposed entities and
whose columns keep the relationship info. To distinguish the ends of the association
you must use the keywords **principal and dependent**. In addition, it's necessary
to denote the muliplicity for each end of the association

```javascript
service { "sample.odata::costumer" as "Costumers";
          "sample.odata::order" as "Orders";
            association "Cutomer_Orders"
                with referential constraint
                principal "Cutomer"("ID") muliplicity "1"
                dependent "Order"("CustomerID") muliplicity "*";
}
```

This association e.g above with the name Costumer_Orders defines a relationship
between the table costumer, identified by its EntitySet name Customers, on the principal end,
and the table order, identified by its entity set name Orders, on the dependent end.
Involved columns of both tables are denoted in braces({}) after the name of the corresponding
entity set. The muliplicity keyword on each end of the association specifies theis cardinality - 
in this example 1..n(one-to-many)

the with referential constraint syntax ensures that the referential constraint check is
enforced at design time, e.g when we activate the service def in the SAP HANA repository.
**The referential constraint info appears at the metadata doc**

```javascript
service { "sample.odata::costumer" as "Costumers"
            navigates ("Costumer_Orders" as "HisOrders");
            
          "sample.odata::order" as "Orders";
            association "Cutomer_Orders"
                with referential constraint
                principal "Cutomer"("ID") muliplicity "1"
                dependent "Order"("CustomerID") muliplicity "*";
}
```

By only defining an association, its not possible to navigate from one entity to another.
Associations need to be bound to entities by a Navigation Property. We can create them by
using the keyword *navigates*
The exmaple above says that it is possible to navigate from Customers over the association
Customer_Order via NavigationProperty named *HisOrders*

so **Associations need to be bound to entities by a Navigation Property**













