# Friends - Ecto Tutorial Practice

## Database Setup [Step 1-8]
  ### Step o1 : Create Project

    mix new friends --sup

  ### Step 02 : Update Dependencies [mix.exs]

    defp deps do
      [
        {:ecto_sql, "~> 3.0"},
        {:postgrex, ">= 0.0.0"}
      ]
    end

  ### Step 03 : Install Dependencies

    mix deps.get

  ### Step 04 : Enable Ecto formatter rules [.formatter.exs]

    [
      import_deps: [:ecto, :ecto_sql],
      inputs: ["{mix,.formatter}.exs", "{config,lib,test}/**/*.{ex,exs}"]
    ]

  ### Step 05 : set up this ecto configuration with generator

    mix ecto.gen.repo -r Friends.Repo
    will create [config/config.exs] [lib/friends/repo.ex]

  ### Step 06 : Edit Database configuration [config/config.exs]

    config :friends, Friends.Repo,
      database: "friends_repo",
      username: "mamuns",
      password: "321",
      hostname: "localhost"

    config :friends, ecto_repos: [Friends.Repo]

  ### Step 07 : add Repo to application's supervision tree

    def start(_type, _args) do
          children = [ Friends.Repo ]

  ### Step 08 : Setting up the database

    mix ecto.create

## Create a table

  ### Step 01 : Create a migration file


    mix ecto.gen.migration create_people

  ### Step 02 : Update migration file

    def change do
        create table(:people) do
          add :first_name, :string
          add :last_name, :string
          add :age, :integer
        end
    end

  ### Step 03 : Run migration to create table in DB

    mix ecto.migrate        # mix ecto.rollback

## Create a Schema [lib/friends/person.ex]

    defmodule Friends.Person do
      use Ecto.Schema

      schema "people" do
        field :first_name, :string
        field :last_name, :string
        field :age, :integer
      end

      def changeset(person, params \\ %{}) do
        person
        |> Ecto.Changeset.cast(params, [:first_name, :last_name, :age])
        |> Ecto.Changeset.validate_required([:first_name, :last_name])
      end
    end

## Insert Data
