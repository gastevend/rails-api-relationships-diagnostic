# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
  Incase you need information pertaining to both tables. it also allows you to
  keep ids on inportant data attributes so that if the data changes the id stays
  the same through out your whole bd structure
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
    Profiles would have many movies in them through the favorites table. like wise
    the movies table would be able to have many profiles through favorites. favorites
    would have a movie_id and a profile_id on the table so that you are able to assign
    one profile many movies as well as you are able to assign one movie many profiles.
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
  has_many :movies, through: :favorites
  has_many :favorites
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
  has_many :profiles, through: :favorites
  has_many :favorites
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
  belongs_to :profiles, inverse_of :favorites
  belongs_to :movies, inverse_of :favorites
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
    The purpose of the Serializer is to be able to customize what comes back as a
    response in JSON from the server.
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
   attributes :id, :given_name, :surname, :email, :movies
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
    bin/rails generate scaffold favorite movie:references profile:references
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
    the dependent destroy tells ruby that if one of the entities on the many to many
    associated tables is destroyed then to also destroy all of the inputs in the through
    table that are associated to that destroyed entity
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
    In a library, a book would have a one to many with authors where a book would
    belongs_to an author. You would also have a many to many relationtion ship with
    readeres who are checking out books. where readers could checkout out many books
    through loans as well as a book could have many checkouts also through loans.
  ```
