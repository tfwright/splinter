# Splinter [![Splinter Build Status][Build Icon]][Build Status]

![](http://i.imgur.com/trnno.jpg)

Splinter is a Capybara Ninja. It provides some helpers to aid with filling in
Rails forms.

Bonus: Splinter also includes a few performance tweaks which can be used to
speed up test suites. These are opt-in.

[Build Icon]: https://secure.travis-ci.org/itspriddle/splinter.png?branch=master
[Build Status]: https://travis-ci.org/itspriddle/splinter

## Installation

    gem 'splinter'
    bundle install

Add to `spec_helper.rb`:

    require 'splinter/rspec'

## Performance Tweaks

Capybara runs in a different thread when the driver is not `rack-test`. This
can cause issues with transactional fixtures as ActiveRecord normal creates a
new connection per-thread. If you need to force ActiveRecord to share the
connection between threads, add the following to `spec_helper`:

    require 'splinter/rspec/shared_connection'

While not technically related to Capybara, the following GC tweak can increase
run time by 10% or more in some suites. To enable it, add the following to
`spec_helper`:

    require 'splinter/rspec/deferred_garbage_collection'

## Screenshots

To capture screenshots on failure, add the following to `spec_helper`:

    Splinter.screenshot_directory = "/path/to/screenshots"

## Date/Time/Datetime Helpers

These are mostly borrowed from [`Hermes::Actions`](http://git.io/bhLQqQ).
They're useful for completing the multiple dropdowns required for a
date/time/datetime field in a Rails form.

    # Select by CSS ID
    select_date Time.now, :id_prefix => :publish_at

    # Select by label text, label must have for="id_prefix"
    select_date Time.now, :from => "Publish at"

There are also `select_time`, and `select_datetime` variants with the same
usage.

## Completing Forms

Here's a little sugar to help completing Rails forms:

    complete_form :post do |f|
      f.text_field :name, "I like turtles!"
      f.date       :publish_at, 3.days.from_now
      f.select     :category, "Blog Posts"
      f.checkbox   :published, false
    end

After the block is evaluated, the form is completed and submitted.

## Note on Patches/Pull Requests

* Fork the project.
* Make your feature addition or bug fix.
* Add tests for it. This is important so I don't break it in a future version
  unintentionally.
* Commit, do not bump version. (If you want to have your own version, that is
  fine but bump version in a commit by itself I can ignore when I pull).
* Send me a pull request. Bonus points for topic branches.

## Copyright

Copyright (c) 2012 Site5.com. See LICENSE for details.