# Hydra RSpec Templates

Collection of sample RSpec test templates

## Using this repo

Download this repo to your workstation and use it as a starting-point for experimenting with different
testing techniques using Hydra.

    git clone https://github.com/awead/hydra-rspec-templates
    bundle install
    bundle exec rake jetty:start
    bundle exec rspec

You will see pending messages and failures.  Look at the README for the topics covered here which includes
linked files of code for you to use as a starting point for setting up your own applications.

## Topics

### Dependencies

#### hydra-jetty and jettywrapper

The hydra-jetty repo contains Solr and Hydra, both deployed via Jetty.  The jettywrapper gem handles downloading,
configuring, starting and stopping an instance of hydra-jetty.

* jettywrapper controls which [release](https://github.com/projecthydra/hydra-jetty/releases) of hydra-jetty you use
* default release is currently v7.0.0
* jettywrapper includes all the rake tasks you need, but you can [override them](lib/tasks/jettywrapper.rake)

#### Capybara

When running your feature tests, the entire page will be rendered as if it's in a browser.  Capybara will
do this and will also test javascript if you provide a driver.

* capybara-screenshot lets you use [save_and_open_page](spec/features/home_page_spec.rb) at any point during a test to see the web page
* poltergeist is a JS driver for capybara

#### Peripheral Gems

* [factory_girl_rails](https://github.com/thoughtbot/factory_girl_rails) - a fixtures replacement with a straightforward definition syntax, support for multiple build strategies...
* [engine-cart](https://github.com/cbeer/engine_cart) - Rake tasks to generate test applications for Rails Engines
* [database_cleaner](https://github.com/DatabaseCleaner/database_cleaner) - Strategies for cleaning databases in Ruby. Can be used to ensure a clean state for testing.
* [equivalent-xml](https://github.com/mbklein/equivalent-xml) - Easy equivalency tests for Nokogiri::XML
* [devise](https://github.com/plataformatec/devise) - Flexible authentication solution for Rails with Warden.
* [byebug (Ruby 2.0 only)](https://github.com/deivid-rodriguez/byebug) -
* [pry](https://github.com/pry/pry) - An IRB alternative and runtime developer console
* [better_errors](https://github.com/charliesome/better_errors) with [binding_of_caller](https://github.com/banister/binding_of_caller) - Retrieve the binding of a method's caller in MRI 1.9.2+
* [webmock](https://github.com/bblimke/webmock) - Library for stubbing and setting expectations on HTTP requests in Ruby.

#### Travis

* testing one or more Rubies
* sending out messages
* using a matrix to speed up the test suite
* installing dependencies
* automated configuration and startup of hydra-jetty

More information at [docs.travis-ci.com](http://docs.travis-ci.com/)

### Testing Principles

#### Narrative Tests

* tests should tell a story
* use `describe`, `context`, `it` and `specify`
* start with [empty tests](spec/narrative_spec.rb) that tell a story
* test output should read (more or less) like plain English

    > "the thing when first created has no name"
    >
    > "the thing when first created its default values includes the person who's creating it"
    >
    > "the thing when first created its default values contains that"
    >
    > "the thing when first created its default values contains this"

#### Fixtures

* create ActiveFedora models and datastreams prior to each test
* delete them when you're done
* use factories for users
* you can use ActiveRecord fixtures
* use fixture files for xml, text or a/v objects

### Examples

#### Setup

Use [spec_helper.rb](spec/spec_helper.rb) to setup your testing environment

* randomize your tests
  - Randomize to catch tests that might be bleeding state into other tests. If your tests randomly fail, look to output of your specs: "Randomized with seed 37226"; Run with `rspec --seed 37226` to rerun the suite with the same conditions.
* load custom support classes and modules
* configure RSpec and extras like Device, DatabaseCleaner and Warden
* clean out Solr and Fedora

#### Models

[sample_afmodel_spec.rb](spec/models/sample_afmodel_spec.rb)

* model composition (datastreams)
* attributes
* default permissions
* required attributes
* solr fields

#### Datastreams

[sample_datastream_spec.rb](spec/models/sample_datastream_spec.rb)

* can selectively test datastreams separately from models
* attributes
* xml comparison using fixtures
* solrization

#### Controllers

[sample_controller_spec.rb](spec/controllers/sample_controller_spec.rb)

#### Views

[sample_view_spec.rb](spec/views/sample_view_spec.rb)

* you'll generally mock and stub everything as opposed to creating ActiveFedora objects

#### Helpers

[sample_helper_spec.rb](spec/helpers/sample_helper_spec.rb)

* use the built-in `helper` method
* mock/stub as needed

#### Features

[Home page feature test](spec/features/home_page_spec.rb)

* executes the real page
* use Capybara matchers
* use Rails path helpers
* don't mock (generally) -- create objects for your tests

#### Routing

[sample_routes_spec.rb](spec/routing/sample_routes_spec.rb)

#### Lib

* catch-all for non-Rails related classes
* does not include any of RSpec's helper methods:
    * `get`, `show` etc. which are used in controllers
    * `rendered` from views
    * `helper` from helpers

#### Supporting Code

[support](spec/support)

* test classes
* helper methods
* any other supplemental code
