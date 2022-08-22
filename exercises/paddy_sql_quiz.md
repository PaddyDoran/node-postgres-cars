# SQL QUIZ

## How to submit this HW

1. Rename this file to `<name>_sql_quiz.md`
2. Put the code you used to answer the question in the triple backticks under each question.

```sql
LIKE this
```

3. Hand your sql dump file in too

## Questions

1. Create a database called 'vets'

```
CREATE DATABASE vets;
```

```
2. List the databases to see if it's there
```

\l;

```
3. Delete it
```

\c postgres /// can't delete if in current db
DROP DATABASE vets;

```
4. Check that it has gone
```

\l;

```
5. Re-create it
```

CREATE DATABASE vets;

```
6. Connect to it and confirm that you are connected
```

psql -h localhost -p 5432 -d vets -U paddydoran

```
7. Create a table called 'owners'. It will have an id for each owner; a name (required), and; an email
```

\conninfo

````
8. Create a table called 'pets'. It will have an id for each pet; a name (required); a type (cat, dog, etc.) a weight (in kg - use 'decimal') column. It will also have an 'owner' field which will have the id of their owner
```sql
CREATE TABLE pets (
  id serial PRIMARY KEY,
  name varchar(255) NOT NULL,
  type varchar(255) NOT NULL,
  weight decimal NOT NULL,
  owner INTEGER,
  constraint fk_owner
     foreign key (owner)
     REFERENCES pets (id)
);
````

9. Alter the pets table to make the type required

```
ALTER TABLE pets
  ALTER COLUMN type SET NOT NULL;
```

```
10. What is the command to inspect the table to see the constraints? Check they are correct.
```

\d pets

```

```

11. Insert the following data

```

```

### Owners

| name | email          |
| ---- | -------------- |
| tom  | tom@yahoo.com  |
| jane | jane@gmail.com |
| adam | adam@yahoo.com |
| andy | andy@gmail.com |

CREATE TABLE owners (
id serial PRIMARY KEY,
name varchar(255) NOT NULL,
email varchar(255) UNIQUE
);
INSERT INTO owners (id, name, email)
VALUES
(1, 'tom', 'tom@yahoo.com'),
(2, 'jane', 'jane@gmail.com'),
(3, 'adam', 'adam@yahoo.com'),
(4, 'andy', 'andy@gmail.com');

### Pets

**Owner's names are shown. You need to link that to their ids**

| name       | type   | weight | owner |
| ---------- | ------ | ------ | ----- |
| tiddles    | cat    | 3.6    | jane  |
| fluffytail | rabbit | 2.1    | tom   |
| fido       | dog    | 23.2   | adam  |
| rover      | dog    | 44.6   | jane  |
| whiskers   | cat    | 4.5    | adam  |
| shergar    | horse  | 600    | tom   |
| squealer   | pig    | 300    | tom   |
| jaws       | shark  | 800    | jane  |

INSERT INTO pets (name, type, weight, owner)
VALUES
('tiddles', 'cat', 3.6, 2),
('fluffytail', 'rabbit', 2.1, 1),
('fido', 'dog', 23.2, 3),
('rover', 'dog', 44.6, 2),
('whiskers', 'cat', 4.5, 3),
('shergar', 'horse', 600, 1),
('squealer', 'pig', 300, 1),
('jaws', 'shark', 800, 2);

````
/// @James is there a better way to do this rather than manually? Could you do a CTE to do this like:
``` with joined_data as (
  SELECT pets.name as pet_name, pets.type as pet_type, pets.weight as pet_weight, owners.id as owner_id
  FROM owners
  JOIN pets ON owners.name = pets.owner
)
INSERT INTO pets (name, type, weight, owner)
VALUES (SELECT * FROM joined_data);


11. What is the command to view the table and its data? Check you've inserted all the data correctly
````

TABLE pets;

```
12. Retrieve all the pets data
```

SELECT \* FROM pets;

```
13. Retrieve the pets data but only name and weight
```

SELECT name, weight FROM pets;

```

14. Retrieve all the pets that weigh less than 20kg
```

SELECT \* FROM pets WHERE weight < 20;

```

15. Retrieve all the pets that weigh more than 20kg and are not a dog
```

SELECT \* FROM pets WHERE weight > 20 AND type != 'dog';

```
16. Count how many pets there are in total
```

SELECT COUNT(\*) FROM pets;

```
17. Show the count by type (e.g. 2 cats, 1 rabbit, etc)
```

SELECT type, COUNT(\*) FROM pets GROUP BY 1 ORDER BY 2 DESC;

```
18. Find the average weight of the dogs and cats
```

SELECT type, AVG(weight) FROM pets GROUP BY 1 WHERE type IN ('dog', 'cat');

```
19. Find the heaviest pet
```

SELECT \* FROM pets ORDER BY weight DESC LIMIT 1;

```
20. Find the lightest pet
```

SELECT \* FROM pets ORDER BY weight ASC LIMIT 1;

```
21. Retrieve all the pets data in reverse name order
```

SELECT \* FROM pets ORDER BY name DESC;

```
22. Increase tiddles's weight to 4kg
```

UPDATE pets SET weight = 4 WHERE name = 'tiddles';

```
23. Remove the shark!
```

DELETE FROM pets WHERE type = 'shark';

```
24. Get the first 2 animals over 20kg
```

SELECT \* FROM pets WHERE weight > 20 LIMIT 2;

```
25. Get the next 2 animals over 20kg
```

SELECT \* FROM pets WHERE weight > 20 OFFSET 2 LIMIT 2;

```

## Advanced (Optional. look [here](https://www.tutorialspoint.com/postgresql/index.html))
26. Join the tables to (effectively) get the pets table above
```

SELECT \* FROM owners INNER JOIN pets ON owners.id = pets.owner;

```

27. Create a table which lists ALL the owners and their respective pets
```

SELECT

- FROM owners
  LEFT JOIN pets ON owners.id = pets.owner;

```

28. Retrieve all the owners who have a gmail email (hint: `in` clause)
```

SELECT \* FROM owners WHERE email LIKE '%gmail%';

```
29. Retrieve all the distinct pet types (hint: look for a keyword)
```

SELECT DISTINCT type FROM pets;

```

** Next 3 are hard!! (Do your own research!!! - Joe Bro-gan) **

30. Create a table which lists the owner's name and the number of pets they own
```

SELECT
owner,
count(\*) as number_of_pets
FROM owner
JOIN pets ON owner.id = pets.owner

```
31. Find the owner who has the biggest cat (hint: sub-query)
```

SELECT

- FROM owners INNER JOIN pets ON owners.id = pets.owner
  WHERE (SELECT pets.type = 'cat'

````
32. Retrieve all the owners who have more than 2 pets (hint: `having` clause)
```sql

````

## Dump/Seed (not optional!)

33. Create an SQL dump of your db (a .sql file created from outside the postgres shell)

```
pg_dump -U postgres -d vets > vets.sql

```

```
34. Delete your db
```

DELETE DATABASE vets;

```
35. Seed from the dump file to re-ceate your database (from outside the postgres shell)
```

psql -U postgres -d vets < vets.sql

```

```
