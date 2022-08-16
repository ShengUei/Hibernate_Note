# Hibernate 中 Save, Persist, Update, Merge, and SaveOrUpdate 的不同

Any entity instance in our application appears in one of the three main states in relation to the Session persistence context:

- transient — This instance isn't, and never was, attached to a Session. This instance has no corresponding rows in the database; it's usually just a new object that we created to save to the database.
- persistent — This instance is associated with a unique Session object. Upon flushing the Session to the database, this entity is guaranteed to have a corresponding consistent record in the database.
- detached — This instance was once attached to a Session (in a persistent state), but now it’s not. An instance enters this state if we evict it from the context, clear or close the Session, or put the instance through serialization/deserialization process.

When the entity instance is in the persistent state, all the changes that we make to the mapped fields of this instance will be applied to the corresponding database records and fields upon flushing the Session. The persistent instance is “online,” whereas the detached instance is “offline” and not monitored for changes.

This means that when we change the fields of a persistent object, we don't have to call save, update, or any of those methods to get these changes to the database. All we need to do is commit the transaction, flush the session, or close the session.

![Simplified State Diagram](https://www.baeldung.com/wp-content/uploads/2016/07/2016-07-11_13-38-11-1024x551.png)

參考資料 : https://www.baeldung.com/hibernate-save-persist-update-merge-saveorupdate
