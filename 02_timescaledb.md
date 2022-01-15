# timescaledb

### setup

1. create migration for timescaledb extension
   ```shell
   mix ecto.gen.migration create_timescaledb_extension
   ```
2. modify generated file

   ```elixir
   defmodule Project.Repo.Migrations.CreateTimescaledbExtension do
       use Ecto.Migration

       def up do
           execute("CREATE EXTENSION IF NOT EXISTS timescaledb CASCADE")
       end

       def down do
           execute("DROP EXTENSION IF EXISTS timescaledb CASCADE")
       end
   end
   ```

### new table(for test, feel free to remove it after practice)

1. create migration for new table
   ```shell
   mix ecto.gen.migration create_test
   ```
2. modify generated file

   ```elixir
   defmodule Project.Repo.Migrations.CreateTest do
        use Ecto.Migration

        def change do
            create table(:test, primary_key: false) do
                add :time, :naive_datetime, null: false
                add :field1, :float, default: 0.0
                add :field2, :string, default: 0.0
                timestamps()
            end
            execute("SELECT create_hypertable('test','time');")
        end
   end
   ```
