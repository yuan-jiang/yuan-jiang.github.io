---
layout: post
title: Upgrade rails from 4.2 to 5.0
date: 2020-05-04 09:00:00 +0900
tags: rails
---

Upgrading rails to a new major version can be tricky because there might be many breaking changes from the framework itself or from many gems used in project that will become either deprecated or unsupported. However, it can also go smooth if done in a right procedural way. This post shares the procedure based on my recent hands-on experience in upgrading rails from `4.2.11` to `5.0.7.2` for our API server. Please be kindly noted that depending on project configuration or dependencies, the upgrading steps might be slightly different for various projects.

## Prerequisites
- Read the official [General Advice](https://guides.rubyonrails.org/upgrading_ruby_on_rails.html) of upgrading rails to get a rough idea on how to proceed and what needs to be done;
- Read the upgrading guide specific to our case: [Upgrading from Rails 4.2 to Rails 5.0](https://guides.rubyonrails.org/upgrading_ruby_on_rails.html#upgrading-from-rails-4-2-to-rails-5-0);
- Confirm ruby version meets requirement, we use ruby `2.4.5` which is fine (Rails 5 requires ruby 2.2.2+);
- Check [all versions of rails](https://rubygems.org/gems/rails/versions), based on the guide from general advice and the current rails version used (`4.2.11`), determine the target version to go: `5.0.7.2`;
- Based on the guides, break down the upgrading process to small tasks and try to limit each task to deal with only one topic.

## Strategies
- Run all rspec tests in the `master` branch and make sure all tests pass;
- Create a new branch based on `master` and name it `rails_upgrade_master` for example;
- For each of the small tasks, branch from `rails_upgrade_master` to deal with one topic at a time;
- Create PR after the task is completed (e.g. breaking code fixed, API replaced with new ones, etc);
- Ask at least 2 other team members to review each PR and merge into `rails_upgrade_master` after approval;
- After all the upgrading tasks are solved and merged into `rails_upgrade_master`:
  + Merge `master` into the `rails_upgrade_master` to catch up if by this time the `master` has been ahead for awhile;
  + Run all rspec tests on `master` again to confirm all tests pass;
  + Run all rspec tests on `rails_upgrade_master` and make notes for the failed tests (in our case, after all previous tasks were solved, there were only 18 failed tests out of 900+ tests);
  + Create tasks to deal with the failed tests and merge into `rails_upgrade_master`;
  + Until all rspec tests pass: congratulations! You're one step away to a successful upgrading;
- Last but not the least, create tasks for regression testing all the major features and ask more people to help in the regression testing (Well if you have QA team, that's another story...):
  + If there are any failed cases found during regression testing, fix them in the `rails_upgrade_master`;
  + Until all regression testing is completed and no more problems found;
- Finally, merge the `rails_upgrade_master` into `master` and cheers!

## Steps
- Update `Gemfile` and `Gemfile.lock`
  + Set `gem 'rails', '~> 5.0.7', '>=5.0.7.2'` in `Gemfile`;
  + Run `bundle update rails` to try updating rails only first:
    + if it succeeds, then congratulations all your gems should work for rails 5;
    + if it fails (in our case) probably due to dependency resolution, then you should run `bundle update` and this would probably upgrade other gems too which also means possible more tasks to deal with breaking changes;
  + After successful dependency resolution, run `bundle install` to make sure `Gemfile` and `Gemfile.lock` both are updated.

- Update configuration and binary files to rails 5
  > This might be the hardest step.
  > Below was the procedure I followed:
  + Create a fresh project in rails `5.0.7.2` with default configuration;
  + Compare content one by one for each of the files under `bin/` and `config/` directories between the current version and the rails 5 fresh project;
  + Note each file for operation such as:
    - To skip: meaning to keep the old version without changes;
    - To override: meaning to replace with new version completely;
    - To merge: meaning to manually merge changes.
  + Now run `rails app:update` and follow what was noted;
  + References:
    - https://guides.rubyonrails.org/upgrading_ruby_on_rails.html#the-update-task
    - https://guides.rubyonrails.org/5_0_release_notes.html#railties-deprecations

- Update models and mailers to follow new conventions
  + Create `ApplicationRecord` as base class and apply for all models;
  + Create `ApplicationMailer` as base class and apply for all mailers.

- Update model relationship changes related to `belongs_to`
  + For all `belongs_to` declarations without `require: false` option, add `optional: true` option;
  + Comment out or set value to `true` in `config/initializers/new_framework_defaults.rb` for `Rails.application.config.active_record.belongs_to_required_by_default` option.

- Update model callback chains halting logic
  + For all models check the `before_xxx` callbacks and for any method that relied on `returning false` to halt the chains, replace with `throw(:abort)`;
  + Set value to `false` in `config/initializers/new_framework_defaults.rb` for `ActiveSupport.halt_callback_chains_on_return_false` option.

- Update existing migration scripts
  + For all existing migrations, update the class definitions to `class SomeMigration < ActiveRecord::Migration[4.2]`;
  + References:
    - https://stackoverflow.com/a/35930912

- Update params in controllers
  + For all params used in controllers, confirm or update so that all mass assignments are protected by strong parameters with `params.require.permit`;
  + For all params used in controllers, confirm or update so that no hash methods (e.g. `map` or `length` and etc.) are called on `params` because `params` is no longer inheriting `HashWithIndifferentAccess`;
  + For all params used in controllers, confirm or update so that all nesting objects and their attributes are also applied with strong parameters protection;
    - https://github.com/rails/rails/issues/9454
    - https://github.com/rails/rails/issues/9454#issuecomment-280853714

- Run all rspec tests against the `rails_upgrade_master` branch
  + Make notes and create tickets to address the failed tests

- Address issues found by failed tests - may be project specific
  + Replace `Sass.compile` with `SassC.new.render` as the former is deprecated in rails 5:
    - https://sass-lang.com/blog/ruby-sass-is-deprecated
    - https://github.com/sass/sassc-rails/issues/105
    - https://github.com/rails/sass-rails/issues/315
    - https://rubygems.org/gems/sass
  + `touch: true` stops working in associated objects, solution: manually set `inverse_of` for `belongs_to` and `has_many` pairs in models associations:
    - https://stackoverflow.com/a/46452718
    - https://github.com/rails/rails/issues/26726
    - https://github.com/rails/rails/issues/7567
    - https://github.com/paper-trail-gem/paper_trail/issues/874
    - https://github.com/rails/rails/pull/29132
    - https://rossta.net/blog/use-inverse-of.html
    - https://www.viget.com/articles/exploring-the-inverse-of-option-on-rails-model-associations/
  + Time comparison related tests failed due to precision; switch to compare time change with `be_within(1.second).of`:
    - https://stackoverflow.com/a/26207378
    - https://github.com/rails/rails/issues/33715
    - https://tddium.wordpress.com/2011/08/07/rails-time-comparisons-devil-details-etc/
    - https://gitlab.com/gitlab-org/gitlab-foss/-/issues/48430
    - https://blog.bigbinary.com/2016/02/23/rails-5-handles-datetime-with-better-precision.html

- Run regression testing and solve failed cases if any - may be project specific
  + Issues related to `ActiveRecord::ConnectionTimeoutError: could not obtain a database connection within 5.000 seconds (waited 5.015 seconds)`:
    - https://stackoverflow.com/questions/38004673/connection-pool-for-rails-5
    - https://qiita.com/katsuyuki/items/42b3c69bcd76c44ad64a
  + Issues related to `skip_callback` in rake tasks:
    - https://github.com/thoughtbot/factory_bot/issues/931
  + Issues related to `<<` or `push` not working for `relation` object:
    - http://techspry.com/ruby_and_rails/active-records-or-push-or-concat-method/
    - https://github.com/rails/rails/issues/25906
    - https://github.com/rails/rails/issues/27998

- Generate and commit `schema.rb` in `rails_upgrade_master`
  + Run `rails db:schema:dump` to generate `schema.rb` with new version;
  + Merge `rails_upgrade_master` into `master`.

- Cheers!