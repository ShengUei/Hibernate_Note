# Hibernate 中 Save, Persist, Update, Merge, and SaveOrUpdate 的不同

Any entity instance in our application appears in one of the three main states in relation to the Session persistence context:

- transient — This instance isn't, and never was, attached to a Session. This instance has no corresponding rows in the database; it's usually just a new object that we created to save to the database.
- persistent — This instance is associated with a unique Session object. Upon flushing the Session to the database, this entity is guaranteed to have a corresponding consistent record in the database.
- detached — This instance was once attached to a Session (in a persistent state), but now it’s not. An instance enters this state if we evict it from the context, clear or close the Session, or put the instance through serialization/deserialization process.

When the entity instance is in the persistent state, all the changes that we make to the mapped fields of this instance will be applied to the corresponding database records and fields upon flushing the Session. The persistent instance is “online,” whereas the detached instance is “offline” and not monitored for changes.

**This means that when we change the fields of a persistent object, we don't have to call save, update, or any of those methods to get these changes to the database. All we need to do is commit the transaction, flush the session, or close the session.**

![Simplified State Diagram](https://www.baeldung.com/wp-content/uploads/2016/07/2016-07-11_13-38-11-1024x551.png)

- Persist
   - The persist method is intended to add a new entity instance to the persistence context, i.e. transitioning an instance from a transient to persistent state.
```java
Person person = new Person();
person.setName("John");
session.persist(person);
```
- What happens after we call the persist method?
    1. The person object has transitioned from a transient to persistent state.
    2. The object is in the persistence context now, but not yet saved to the database.
    3. The generation of INSERT statements will occur only upon committing the transaction, or flushing or closing the session.


- Save
  - Its purpose is basically the same as persist, but it has different implementation details.
  - The documentation for this method strictly states that it persists the instance, “first assigning a generated identifier.”
  - The method will return the Serializable value of this identifier


- Update
  - it acts upon a passed object (its return type is void). The update method transitions the passed object from a detached to persistent state.
  - this method throws an exception if we pass it a transient entity.
  
**The method name update is a bit misleading here. It does not mean that an SQL UPDATE is immediately performed.**



參考資料 : https://www.baeldung.com/hibernate-save-persist-update-merge-saveorupdate
