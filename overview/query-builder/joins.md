# Joins

## Inner Join Clause

The query builder may also be used to write join statements. To perform a basic "inner join", you may use the `join` method on a query builder instance. The first argument passed to the `join` method is the name of the table you need to join to, while the remaining arguments specify the column constraints for the join. Of course, as you can see, you can join to multiple tables in a single query:

```text
//qb
var getResults = query.from('blogs')
    .join('users', 'users.ID', '=', 'blogs.FK_usersID')
    .get();
writeDump(getResults);

//sql
SELECT users.name,blogs.title,blogs.ID as blogID FROM `blogs` INNER JOIN `users` ON `users`.`ID` = `blogs`.`FK_usersID`
```

### Complex (multi-conditional) Join Clause

For a compound join clause, pass in the name of the table as the first argument (just as before) but instead of passing the remaining arguments descirbing the single join clause, we'll pass a single closure with a `joinClause` argument. Consider a (contrived) example where our `users` and `blogs` had to match not only `ID` but also `type`:

```text
//qb
var getResults = query.from('blogs')
    .join('users', function( clauseObj ) {
        return clauseObj.on('users.ID','=','blogs.FK_usersID')
        .on('users.type','=','blogs.type')
    })
    .get();

//sql
SELECT users.name,blogs.title,blogs.ID as blogID FROM `blogs` INNER JOIN `users` ON `users`.`ID` = `blogs`.`FK_usersID` AND `users`.`type` = `blogs`.`type`
```

## Left/Right Join Clause

If you would like to perform a "left/right join" instead of an "inner join", use the `leftJoin` / `rightJoin` method. The `leftJoin` / `rightJoin` method has the same signature as the `join` method:

```text
//qb (leftJoin)
var getResults = query.from('blogs')
    .leftJoin('users', 'users.ID', '=', 'blogs.FK_usersID')
    .get();
writeDump(getResults);

//qb (rightJoin)
var getResults = query.from('blogs')
    .rightJoin('users', 'users.ID', '=', 'blogs.FK_usersID')
    .get();
writeDump(getResults);

//sql (leftJoin)
SELECT * FROM `blogs` LEFT JOIN `users` ON `users`.`ID` = `blogs`.`FK_usersID`

//sql (rightJoin)
SELECT * FROM `blogs` RIGHT JOIN `users` ON `users`.`ID` = `blogs`.`FK_usersID`
```

## Cross Join Clause

To perform a "cross join" use the `crossJoin` method with the name of the table you wish to cross join to. Cross joins generate a cartesian product between the first table and the joined table:

```text
var getResults = query.from('users')
    .crossJoin('departments', 'departments.ID', '=', 'users.FK_departmentID')
    .get();
writeDump(getResults);
```

