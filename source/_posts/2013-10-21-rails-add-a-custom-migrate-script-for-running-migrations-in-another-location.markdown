---
layout: post
title: "Rails: Add a custom migrate script for running migrations in another location."
date: 2013-10-21 19:14
comments: true
categories: [rails,rake,migrations]
---

I am currently in the process of refactoring some code in a Rails project for a client, and one of the things that I wanted to do was to separate the database migration scripts that are from the base frameworks (Discourse was used as the base) and the ones I will be adding. This will give a good historical view of all the custom migrations so we don't have to go through each one. But in order to do that I had to write my own custom rake task to rake the rake tasks:

```
namespace :db do
    desc "Migrate the database by going through the standard migrate folder first, then the custom 1017 folder (options: VERSION=x, VERBOSE=false, SCOPE=blog)."
    task :custom_migrate => [:environment, :load_config] do
        ActiveRecord::Migration.verbose = ENV["VERBOSE"] ? ENV["VERBOSE"] == "true" : true
        ActiveRecord::Migrator.migrate(ActiveRecord::Migrator.migrations_paths, ENV["VERSION"] ? ENV["VERSION"].to_i : nil) do |migration|
            ENV["SCOPE"].blank? || (ENV["SCOPE"] == migration.scope)
        end
        ActiveRecord::Migrator.migrate('db/migrate/1017', ENV["VERSION"] ? ENV["VERSION"].to_i : nil) do |migration|
            ENV["SCOPE"].blank? || (ENV["SCOPE"] == migration.scope)
        end

        Rake::Task['db:_dump'].invoke
    end
end
```

Notice the last line where it delegates out to the ```db:_dump``` task. This is important as it writes out the schema as the end step. 
