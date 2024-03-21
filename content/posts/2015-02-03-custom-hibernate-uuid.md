---
title: Hibernate Custom UUID generator
description: "Use a custom uuid generator in Hibernate"
date: 2015-02-03
draft: false
toc: false
tags:
  - java
  - hibernate
---

### Use a custom uuid generator in Hibernate.
Many web applications these days use UUIDs as primary keys in their database. While this approach has its pros and cons (see [Primary Keys vs GUIDs](http://blog.codinghorror.com/primary-keys-ids-versus-guids/)), one of the principal advantages is that you can generate these ids outside of the database.

### Contrived Example
*System Alpha* sends messages to *System Beta* which results in an entity being created in *System Gamma*. *Alpha* later needs to query *Gamma* for that entity.

- One (horribly-inefficient) way to do this would be for *Alpha* to wait for *Beta* which waits for *Gamma* to come back with an ID that is then passed up. You probably wouldn't print this code and stick it on the refrigerator :-)
- A better way to do this would be for *Alpha* to generate a UUID and pass that along to *Beta* which uses that UUID in its interaction with *Gamma*. *Gamma* would create that entity with the given UUID and *Alpha* could query *Gamma* at leisure.

### So What?
Well, assume you have the following in *Gamma* to create an entity (Gist [here](https://gist.github.com/rajivrnair/2f660011d821002ecc4c)):

~~~ java
@Id
@Column(name = "entity_id")
@GeneratedValue(generator = "system-uuid")
@GenericGenerator(name = "system-uuid", strategy = "uuid2")
private String entityId;
~~~

This will generate a UUID for an entity when it is created. The fly in the ointment is getting hibernate to use the uuid passed in, whenever there is one or fall back to the default behaviour of generating one if the UUID not passed in. You can do this by creating a custom id generator in Hibernate like so:

~~~ java
/**
 * Based on http://stackoverflow.com/a/5392349/566434
 */
public class InquisitiveUUIDGenerator extends UUIDGenerator {

    private String entityName;

    @Override
    public void configure(Type type, Properties params, Dialect dialect) {
        entityName = params.getProperty(ENTITY_NAME);
        super.configure(type, params, dialect);
    }

    @Override
    public Serializable generate(SessionImplementor session, Object object) {
        Serializable id = session
                .getEntityPersister(entityName, object)
                .getIdentifier(object, session);

        if (id == null) {
            return super.generate(session, object);
        } else {
            return id;
        }
    }
}
~~~

As you can see, this uses the UUID passed in or generates one if the id is absent. The original entity class too needs to change like so:

~~~ java
@Id
@Column(name = "entity_id")
@GeneratedValue(generator = "inquisitive-uuid")
@GenericGenerator(name = "inquisitive-uuid", strategy = "com.myapp.persistence.generators.InquisitiveUUIDGenerator")
private String entityId;
~~~

### References
These people have written a lot of sensible stuff about UUIDs and Hibernate identifiers.

- [Hibernate and UUID Identifiers](http://vladmihalcea.com/2014/07/01/hibernate-and-uuid-identifiers/)
- [Identity Vs. Uniqueidentifier (Newbie question)](https://groups.google.com/forum/?hl=en#!msg/microsoft.public.sqlserver.programming/qtCRhLLM9Kk/tg9vDfjbYW0J)
- [Primary Keys: IDs vs GUIDs](http://blog.codinghorror.com/primary-keys-ids-versus-guids/)
