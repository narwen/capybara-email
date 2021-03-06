# CapybaraEmail #

[![Build Status](http://travis-ci.org/dockyard/capybara-email.png)](http://travis-ci.org/dockyard/capybara-email)

Easily test [ActionMailer](https://github.com/rails/rails/tree/master/actionmailer) and [Mail](https://github.com/mikel/mail) messages in your Capybara integration tests

## Installation ##

Add this line to your application's Gemfile:

```ruby
gem 'capybara-email'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install capybara-email

## Usage ##

### RSpec ###

Require `capybara/email/rspec` in your `spec_helper`

Example:

```ruby
feature 'Emailer' do
  background do
    # will clear the message queue
    clear_emails
    visit email_trigger_path
    # Will find an email sent to test@example.com
    # and set `current_email`
    open_email('test@example.com')
  end

  scenario 'following a link' do
    current_email.click_link 'your profile'
    page.should have_content 'Profile page'
  end

  scenario 'testing for content' do
    current_email.should have_content 'Hello Joe!'
  end

  scenario 'view the email body in your browser' do
    # the `launchy` gem is required
    current_email.save_and_open
  end
end
```

### Cucumber ###
Require `capybara/email` in your `features/support/env.rb`

    require 'capybara/email'

Once you have required `capybara-email`, gaining access to usable methods
is easy as adding this module to your Cucumber `World`:

    World(Capybara::Email::DSL)

I recommend adding this to a support file such as `features/support/capybara_email.rb`

```ruby
require 'capybara/email'
World(Capybara::Email::DSL)
```

Example:

```ruby
Scenario: Email is sent to winning user
  Given "me@example.com" is playing a game
  When that user picks a winning piece
  Then "me@example.com" receives an email with "You've Won!" as the subject

Then /^"([^"]*)" receives an email with "([^"]*)" as the subject$/ do |email_address, subject|
  open_email(email_address)
  current_email.subject.should == subject
end
```

### Test::Unit ###

Include `Capybara::Email::DSL` in your test class

```ruby
class ActionController::IntegrationTest
  include Capybara::Email::DSL
end
```

Example:

```ruby
class EmailTriggerControllerTest < ActionController::IntegrationTest
  def setup
    # will clear the message queue
    clear_emails
    visit email_trigger_path

    # Will find an email sent to `test@example.com`
    # and set `current_email`
    open_email('test@example.com')
  end

  test 'following a link' do
    current_email.click_link 'your profile'
    page.should have_content 'Profile page'
  end

  test 'testing for content' do
    current_email.should have_content 'Hello Joe!'
  end

  test 'view the email body in your browser' do
    # the `launchy` gem is required
    current_email.save_and_open
  end
end
```

## Authors ##

[Brian Cardarella](http://twitter.com/bcardarella)

## Versioning ##

This gem follows [Semantic Versioning](http://semver.org)

## Want to help? ##

Stable branches are created based upon each minor version. Please make
pull requests to specific branches rather than master.

Please make sure you include tests!

Unless Rails drops support for Ruby 1.8.7 we will continue to use the
hash-rocket syntax. Please respect this.

Don't use tabs to indent, two spaces are the standard.

## Legal ##

[DockYard](http://dockyard.com), LLC &copy; 2012

[@dockyard](http://twitter.com/dockyard)

[Licensed under the MIT license](http://www.opensource.org/licenses/mit-license.php)
