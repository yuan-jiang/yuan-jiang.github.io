---
layout: post
title: Upgrade rails from 5.0 to 5.1
date: 2020-09-06 12:00:00 +0900
tags: rails ruby
---

Upgrading application rails version from `5.0` to `5.1` is easier than doing [from 4.2 to 5.0](/upgrade-rails-from-4.2-to-5.0), since it's only a minor version update. However, there might still be quite a few things to address either from framework changes or due to outdated gems dependency.

## Config defaults
Since rails 5.1, it is needed to specify default configuration of which version should be loaded by default:
{% highlight ruby %}
config.load_defaults 5.1
{% endhighlight %}
This means the app will load default configuration of rails 5.1 and it's earlier versions (i.e. `<= 5.0`) that are listed in below files:
- `config/initializers/new_framework_defaults.rb` (generated when upgrading from 4.2 to 5.0)
- `config/initializers/new_framework_defaults_5_1.rb` (generated when upgrading from 5.0 to 5.1)

Tips: if there are custom configuration (means the opposite value of default), they can be moved to `config/application.rb` or the specific `config/environment/*.rb` files. After that, such `new_framework_defaults_*` files can be removed completely.

See [Upgrading Rails: What am I to do with new_framework_defaults file?](https://stackoverflow.com/a/51275459).

## Manifest.js file
Sprockets will be upgraded to version 4.x in rails 5.1 and it requires a `manifest.js` file to live under `app/assets/config` otherwise it raises error to complain. See solution [here](https://github.com/rails/sprockets-rails/issues/444#issuecomment-548526846).

## Parameters.with_indifferent_access
The `with_indifferent_access` method of `params` is removed in rails5.1, so if there is code that still relies on this method such as user registration related features based on `devise`, the params must be first permitted and then converted into a hash:
{% highlight ruby %}
User.invite! params[:user].permit(:fullname, :email).to_h, current_user
{% endhighlight %}

## Tons of deprecation warnings from `changed{_xxx}` related methods
{% highlight plain %}
DEPRECATION WARNING: The behavior of `changed_attributes` inside of after callbacks will be changing in the next version of Rails. The new return value will reflect the behavior of calling the method after `save` returned (e.g. the opposite of what it returns now). To maintain the current behavior, use `saved_changes.transform_values(&:first)` instead. (called from block (6 levels) in <top (required)> at
...
{% endhighlight %}
Here is the rails [PR](https://github.com/rails/rails/pull/25337) and a good [post](https://www.fastruby.io/blog/rails/upgrades/active-record-5-1-api-changes). 

Tips: Don't fully trust the deprecation warnings message about `it impacts only inside of after callbacks`, make and apply the changes to all `before_`, `after_` callbacks including `validate`.

If the app still uses an old version of `paper_trail` (e.g. v4.x), it's likely to add handreds of thousands of more such deprecation warnings. Here is the [issue and solution](https://github.com/paper-trail-gem/paper_trail/issues/974). 

## Bigint as primary key for db tables
Rails 5.1 uses bigint for primary key in db tables and the `db:load:schema` (e.g in `test` environment) command will fail if the `schema.rb` file was generated with an earlier version. See [PR](https://github.com/rails/rails/pull/26266).

Tips: If the app is not another `twitter`, you can save the risk of changing db columns in production and keep using `integer` as primary key for exsiting tables. Just dump a version of `schema.rb` from existing production with `db:schema:dump` and then commit into the rails 5.1 branch and the `schema.rb` will have a `id: :integer` attribute for `create_table` command for existing tables. See this post: [Rails 5.1 new default BIGINT primary key sort of ruins existing apps (with fix!)](https://ridingtheclutch.com/post/160099545985/rails-51-new-default-bigint-primary-key-sort-of).

## Summary
The above lists only the main hassles. Others like deprecation warnings about using string for `:if` and `:unless` conditions or config like `ActiveSupport.halt_callback_chains_on_return_false` and so on are generally easy to address: just follow the instruction.
