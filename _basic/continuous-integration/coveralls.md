---
title: Integrating Codeship Basic With Coveralls for Code Coverage Reports
layout: page
tags:
  - analytics
  - code coverage
  - coverage
  - reports
  - reporting
menus:
  basic/ci:
    title: Using Coveralls
    weight: 7
redirect_from:
  - /analytics/coveralls/
  - /classic/getting-started/coveralls/
  - /basic/analytics/coveralls/
---

* include a table of contents
{:toc}

## Coveralls Discount Code

Thanks to our partnership with Coveralls we can provide a 25% Discount for 3 months. Use the code **"coverallslovescodeship"** and [get started right away](https://coveralls.io/).

## Setting Up Coveralls

### Setting Your Coveralls Variables

Starting with Coveralls and Codeship is easy. [Their documentation](https://coveralls.io) do a great job of guiding you, but all there is to set up your Ruby app is add a .coveralls.yml file to your codebase that contains your Coveralls key:

```
repo_token: YOUR_COVERALLS_TOKEN
```

It is also possible to set this in the [environment variables]({{ site.baseurl }}{% link _basic/builds-and-configuration/set-environment-variables.md %}) for your project.

You can do this by navigating to _Project Settings_ and then clicking on the _Environment_ tab.

### Coveralls Gem

Next, you'll need to require the Gem in your Gemfile.

```
gem 'coveralls', require: false
```

### Application Configuration

Now, you'll need to put the Covealls initializers into your `spec_helper.rb` or `env.rb` file, depending on which framework you use.

```ruby
require 'coveralls'
Coveralls.wear!
```

If you want to combine the coverage data from different frameworks, add the following to your `spec_helper.rb` or `env.rb`.

```ruby
# Coveralls with Rspec and Cucumber
require 'coveralls'
Coveralls.wear_merged!
SimpleCov.merge_timeout 3600

# MAKING SURE SIMPLECOV WORKS WITH THE PARALLEL_TESTS GEM
SimpleCov.command_name "RSpec/Cucumber:#{Process.pid.to_s}#{ENV['TEST_ENV_NUMBER']}"
```

Then you need to add a rake task that pushes your coverage report as soon as your build is finished.

```ruby
require 'coveralls/rake/task'
Coveralls::RakeTask.new
```

### Pushing Data

To push the data to Coveralls, add the following after your [test commands]({{ site.baseurl }}{% link _basic/quickstart/getting-started.md %}) on Codeship:

```ruby
bundle exec rake coveralls:push
```

### Setup for other languages

Coveralls supports a lot of other languages. Check out their fantastic [documentation](https://coveralls.io/docs/supported_continuous_integration).
