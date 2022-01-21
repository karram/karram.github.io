# SQLAlchemy

# What is it ?
- SQLAlchemy is the most popular ORM in python world
- It lets you model data as classes (Object) and map them to databases (relational)
- It also talks to multiple DB types such as ms-sql, sqlite, postgres etc and handles the different sql dialects automatically
- Adapts itself to different DBs just based on the connection string !
- Open sourced

# Who uses it ?
- Dropbox
- Uber
- Reddit (not ORM though, only the core)
- Mozilla
- Openstack
- Freshbooks
- Hulu
- Yelp

# Why use it ?
- Using the ORM is not mandatory
- Mature, very fast and has been around for a while. Performance sensitive parts are implemented in C
- DBA Approved - Lets you swap in default generated sql with hand-optimized sql statements (provided by a DBA, for example)
- Unit of Work model - Organizes pending operations into queues and commits them all at once (or not, if needed)
-- (As opposed to active record, where every piece of DB operation is committed separately).
Eager loading - load related objects (through collections and references) and return a graph. This improves performance drastically and reduces DB operations

# Architecture
- SA is built on top of python DB API
- SA Core lets you work close to the sql layer. Uses an intermediate sql-like query language that automatically gets converted into the various dialects

# Using SA
__Package name__: sqlalchemy


**Note**: Code snippets below assume sqlalchemy is imported as sa


Objects modelled as python classes. All objects need a base class, and objects in the same db have the same base class

use "**__tablename__**" to specify the name of the table for any class.
define a column with sa.Column
define the object type with predefined values supplied by SA. For example,

    id = sa.Column(sa.String, primary_key=True)
