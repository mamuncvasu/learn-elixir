## Elixir Basics

## Ecto Getting Started Practice all
### Friends [ https://hexdocs.pm/ecto/getting-started.html ]

    mix new friends --sup   # create project in supervisor mode

### edit: mix.exs
    defp deps do
      [
        {:ecto_sql, "~> 3.0"},
        {:postgrex, ">= 0.0.0"}
      ]
    end
### install dependencies
    mix deps.get
### edit .formatter.exs file
    [
      import_deps: [:ecto, :ecto_sql],
      inputs: ["{mix,.formatter}.exs", "{config,lib,test}/**/*.{ex,exs}"]
    ]

### create configuration for ecto
  mix ecto.gen.repo -r Friends.Repo

### edit config/config.exs with DB details and add 1 line to tell about repo
    config :friends, ecto_repos: [Friends.Repo]

### edit lib/friends/application.ex

    def start(_type, _args) do
      children = [
        Friends.Repo,
      ]

### Create Database
    mix ecto.create

### create a migration [priv/repo/migrations/..]
    mix ecto.gen.migration create_people

### edit that file with table details
    def change do
      create table(:people) do
        add :first_name, :string
        add :last_name, :string
        add :age, :integer
      end
    end
### run migration to create table
    mix ecto.migrate
    mix ecto.rollback  # if want delete that table

### create schema [lib/friends/person.ex]
    defmodule Friends.Person do
      use Ecto.Schema

      schema "people" do
        field :first_name, :string
        field :last_name, :string
        field :age, :integer
      end
    end

### Open in iex
    iex -S mix
### create a entry but not saved yet

    person = %Friends.Person{} # new Person with empty data
    person = %Friends.Person{age: 28}  # new Person with age only
    person = %Friends.Person{first_name: "Ryan", last_name: "Rigg"} # Person with 2 params
    or, person = %{person | age: 28}

    person.age # => 28

### Save a create person in DB
    Friends.Repo.insert(person)

### Validation 
    add to lib/friends/person:
    def changeset(person, params \\ %{}) do
      person
      |> Ecto.Changeset.cast(params, [:first_name, :last_name, :age])
      |> Ecto.Changeset.validate_required([:first_name, :last_name])
    end
### Using validation to check validity [ iex -S mix ]

    # error khabe
    person = %Friends.Person{}
    changeset = Friends.Person.changeset(person, %{})
    Friends.Repo.insert(changeset)

    person = %Friends.Person{}
    changeset = Friends.Person.changeset(person, %{first_name: "Ryan", last_name: "Bigg"})
    Friends.Repo.insert(changeset)

    changeset.valid? # true/false
    changeset.errors

### Insert Multiple records
    people = [
      %Friends.Person{first_name: "Ryan", last_name: "Bigg", age: 28},
      %Friends.Person{first_name: "John", last_name: "Smith", age: 27},
      %Friends.Person{first_name: "Jane", last_name: "Smith", age: 26},
    ]

    Enum.each(people, fn (person) -> Friends.Repo.insert(person) end)

### Remove exiting repo(DB) and create again
    mix ecto.drop
    mix ecto.create
    mix ecto.migrate

### Fetching a single record without condition
    # First1 ta data anbe kintu show korbe na
    Friends.Person |> Ecto.Query.first 

    # first 1ta data show korbe
    Friends.Person |> Ecto.Query.first |> Friends.Repo.one 

### Fetching a single record with id or any attribute
    Friends.Person |> Friends.Repo.get(1) # where id: 1
    Friends.Person |> Friends.Repo.get_by(first_name: "Ryan") # where first_name: "Ryan"

### Fetch all data

    Friends.Person |> Friends.Repo.all

### Filtering results

    # get multiple records matching a specific attribute
    Friends.Person |> Ecto.Query.where(last_name: "Smith") |> Friends.Repo.all
    or, Ecto.Query.from(p in Friends.Person, where: p.last_name == "Smith") |> Friends.Repo.all

    # same query using variable
    last_name = "Smith"
    Friends.Person |> Ecto.Query.where(last_name: ^last_name) |> Friends.Repo.all

### Updating Record

    person = Friends.Person |> Ecto.Query.first |> Friends.Repo.one
    changeset = Friends.Person.changeset(person, %{age: 29})
    Friends.Repo.update(changeset)

### Delete Record

    # way 01
    person = Friends.Person |> Friends.Repo.get(1)
    Friends.Repo.delete(person)
    # Way 02
    person = Friends.Repo.get(Friends.Person, 1)
    Friends.Repo.delete(person)

### Basic CRUD cheatsheet [https://hexdocs.pm/ecto/crud.html]


### তুমি % ব্যবহার করছ কারণ:

    এটি Elixir-এ struct instantiating তৈরির উপায়। অনেকটা OOP Class এর instance তৈরির মতো { Person.new }
    Example: 
    defmodule Friends.Person do
      use Ecto.Schema

      schema "people" do
        field :name, :string
        field :age, :integer
      end
    end

    # This creates a new Person struct with default nil values:
    person = %Friends.Person{}
