### Improving Query Performance with Database Indexes

How to index on multiple columns in a table and how that benefits performance in
database queries.

Keep an eye on how many indecies your adding because:

1. They eat up more storage space.
2. They slow down ```INSERT``` and ```UPDATE``` in the database.

This is because an index improves ```READ``` but slows down ```WRITE```.
