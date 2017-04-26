# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
    It can store relation data from various tables and then display information relavant to the task at hand while minimizing the amount of data stored in other tables.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
    table profiles:
    will have an an indexed id, given_name is a TEXT field, surname is a TEXT field, and email is a TEXT field.

    table movies:
    will have an indexed id, title is a TEXT field, release_date is a DATE field, and length is an INTEGER.

    table favorites:
    will have an indexed id, movie_id which references movies.id, and profile_id which references profiles.id.

    1 Profile to Many Favorites
    1 Movie to Many Favorites

  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base

  has_many :favorites, dependedent: :destory
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
  has_many: favorites, dependedent: :destroy
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
  belongs_to: :profile, inverse_of: :favorites
  belongs_to: :movie, inverse_of: :favorites
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
    The Serializer allows the filtered display of data otherwise without it, all column fields and their data will be displayed which may not be needed for the task at hand.
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :id, :given_name, :surname, :favorites, :title
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
    bin/rails generate scaffold Favorite profile_id:index movie_id:index
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
    It will remove entries where the deleted ID exists so that there are no "hanging" instances of that ID. It would be defined in the relevant model files after a has_many definition.
    e.g. for profile
    has_many :favorites, dependedent: :destory
    This will remove the joined profile id instances from the favorites table.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
    Outside of movies,
    for a doctor and patient:
    one doctor to many patients, e.g. primary care
    many patients to many doctors, e.g. patient seeing/having many doctors with specializations.
  ```
