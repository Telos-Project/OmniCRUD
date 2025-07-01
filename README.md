# OmniCRUD

## 1 - Abstract

***The State of Union.***

OmniCRUD is a series of JSON formats and conventions for state serialization and for simultaneous
querying across diverse databases.

## 2 - Contents

### 2.1 - Conventions

#### 2.1.1 - Format Objects

A format object is a JSON object which contains two fields, "format", containing a string
specifying the alias of said format, and "content", containing a string, object, or list specifying
content in said format.

Optionally, a format object may have an additional "properties" field, containing a miscellaneous
value specifying additional information about the format object.

Environments may support various formats, assigning each an alias. Some environments may restrict
certain formats for security reasons.

#### 2.1.1.1 - Selectors

Format objects may be used to specify selectors for hierarchical data structures.

Such formats for JSON are listed below, in order from most safe to least safe:

- [JMESPath](https://jmespath.org/)
- [JSONata](https://jsonata.org/)
- [JSONPath](https://goessner.net/articles/JsonPath/)
- [JSONPath-Plus](https://github.com/JSONPath-Plus/JSONPath)

#### 2.1.1.2 - Transforms

Format objects may be used to specify transformations for data structures.

#### 2.1.2 - Meta JSON

Meta JSON is a JSON object format for assigning properties to JSON content without embedding said
properties into the content itself.

A meta JSON object has two fields, "content", containing a JSON object or list, and "metadata",
containing a list of metadata objects. Metadata objects have two fields, "selector", containing a
JSON selector format object for selecting values within the aforementioned content, and
"properties", containing a miscellaneous value to associate with the selected values.

### 2.2 - OmniCRUD

#### 2.2.1 - JSON

##### 2.2.1.1 - JSON Database Mapping

The content and structure of a database, be it relational or NoSQL, may be partially or wholly
mapped to JSON, with the properties of database and dataset (table, collection, etc.) assigned via
meta JSON.

Such a database may be called a JSON mapped database.

##### 2.2.1.2 - Target JSON

A target JSON object is a JSON object with the fields "selector", containing a JSON selector format
object specifying specific values within a JSON mapped database, and "address", containing a string
specifying the address or alias of a database containing said values. Optionally, a JSON query
object may have a "properties" field, containing a miscellaneous value which, among other things,
may serve to specify access permissions.

##### 2.2.1.3 - JSON Queries

A JSON query is a JSON object with the fields "type", containing a string specifying the operation
(as "create", "read", "update", or "delete"), and "target", containing a target JSON object
specifying the alias or address of the database to enact the query on. Optionally, a JSON query
object may have a "properties" field, containing a miscellaneous value.

An update query shall also have the field "transform", containing a JSON transform format object to
apply to the selected values.

A create field shall also have the field "content", containing a JSON value to assign or append to
the selected values.

JSON queries may be submitted as lists of JSON query objects.

#### 2.2.2 - State

##### 2.2.2.1 - Subscriptions

OmniCRUD subscriptions event handlers tied to specific values within a JSON mapped database which
trigger events when said values are altered, ideally with the previous and new values being passed
to said event.

OmniCRUD subscriptions may be specified with target JSON objects.

##### 2.2.2.2 - Entanglement

OmniCRUD entanglement is when rules for two values in separate JSON mapped databases are enforced
to keep them aligned, if not identical, when one of them is altered.

OmniCRUD entanglements may be specified with target JSON objects, handled by OmniCRUD
subscriptions, and declared and unidirectional or bidirectional.

#### 2.2.3 - Applications

##### 2.2.3.1 - Agnostic Scripts

Agnostic scripts are scripts for a system which may be written in any language. As such, rather
than interacting with said system through environmental variables and functions, the scripts,
written as function bodies, return an OmniCRUD query as a JSON string, which executes upon the
state of said system (said state itself represented as JSON), the results of which are then passed
to the same script as a JSON string upon its next execution.