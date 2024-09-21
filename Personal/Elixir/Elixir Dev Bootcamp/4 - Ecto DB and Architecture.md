What does insert! vs insert do?
! functions return errors and non ! functions return status tuples

Does unique_constraint get checked before or during our hit to the database?
During

Why is it important to only access the database in outside modules via a context module?
It separates concerns where the context are concerned with the "how to query postgres"

What does a Context Module do?
Allow you to group schema definitions and queries

What is the difference between `from` queries and `where`?
Individual functions are composable

Why is it important to use ^ in this case:

price = 30
from h in House, where: h.price > ^price

^ defines that it's using a variable not within query scope

Working with Relations in Ecto


What does cast_assoc do?
Allow you to create or update relational fields

When does preload actually query for your relations?
It queries for the relations after the first query runs


What's the correct syntax for adding a migration for a belongs_to relationship?
create table(:table_name) do
  add :user_id, references(:users)
end

Why is Ecto Shorts useful?
It shortens code and allows for many filters without much code

What do we need to update when generating schemas?
Relationships inside of the schema module

How can we easily create ecto code and integrate it with a REST api?
Generators

What's the correct syntax for dataloader?
resolve: dataloader(MyContext, :schema)