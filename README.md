[![Gem Version](https://badge.fury.io/rb/active_record_survey.svg)](http://badge.fury.io/rb/active_record_survey)
[![Build Status](https://travis-ci.org/butchmarshall/active_record_survey.svg?branch=master)](https://travis-ci.org/butchmarshall/active_record_survey)

# ActiveRecordSurvey

An attempt at a more versatile data structure for making and taking surveys.

This gem tries to be as unopinionated as possible on the peripheral details on how you implement a survey.

The goal is to give a simple interface for creating surveys and validating the answers given to them.

Release Notes
============

**0.1.22**
 - answer#remove_link cleaned up so it can be understood

**0.1.20**
 - answer#build_link now detects and throws an error when a infinite loop is added

**0.1.20**
 - answer#remove_link wasn't correct.  Fixed and added unit tests

**0.1.15**
 - Don't consider instance_nodes marked for destruction when validating

**0.1.14**
 - build_question now only accepts a question
 - Exceptions added

**0.1.13**
 - Added Question#build_answer and Answer#build_link to make survey creation possible without dealing with internal nodes

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'active_record_survey'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install active_record_survey

## Installation

```ruby
rails generate active_record_survey:active_record
```

## Usage

See the spec file for more detailed usage.

### Build a basic survey
```ruby

# New method for building surveys
@survey = ActiveRecordSurvey::Survey.new()

@q1 = ActiveRecordSurvey::Node::Question.new(:text => "Question #1")
@survey.build_question(@q1)
@q1_a1 = ActiveRecordSurvey::Node::Answer.new(:text => "Q1 Answer #1")
@q1_a2 = ActiveRecordSurvey::Node::Answer.new(:text => "Q1 Answer #2")
@q1_a3 = ActiveRecordSurvey::Node::Answer.new(:text => "Q1 Answer #3")
@q1.build_answer(@q1_a1, @survey)
@q1.build_answer(@q1_a2, @survey)
@q1.build_answer(@q1_a3, @survey)

@q2 = ActiveRecordSurvey::Node::Question.new(:text => "Question #2")
@survey.build_question(@q2)
@q2_a1 = ActiveRecordSurvey::Node::Answer.new(:text => "Q2 Answer #1")
@q2_a2 = ActiveRecordSurvey::Node::Answer.new(:text => "Q2 Answer #2")
@q2.build_answer(@q2_a1, @survey)
@q2.build_answer(@q2_a2, @survey)

@q3 = ActiveRecordSurvey::Node::Question.new(:text => "Question #3")
@survey.build_question(@q3)
@q3_a1 = ActiveRecordSurvey::Node::Answer.new(:text => "Q3 Answer #1")
@q3_a2 = ActiveRecordSurvey::Node::Answer.new(:text => "Q3 Answer #2")
@q3.build_answer(@q3_a1, @survey)
@q3.build_answer(@q3_a2, @survey)

@q4 = ActiveRecordSurvey::Node::Question.new(:text => "Question #4")
@survey.build_question(@q4)
@q4_a1 = ActiveRecordSurvey::Node::Answer.new(:text => "Q4 Answer #1")
@q4_a2 = ActiveRecordSurvey::Node::Answer.new(:text => "Q4 Answer #2")
@q4.build_answer(@q4_a1, @survey)
@q4.build_answer(@q4_a2, @survey)

# Link up Q1
@q1_a1.build_link(@q2)
@q1_a2.build_link(@q3)
@q1_a3.build_link(@q4)

# Link up Q2
@q2_a1.build_link(@q4)
@q2_a2.build_link(@q3)

# Link up Q3
@q3_a1.build_link(@q4)
@q3_a2.build_link(@q4)

# Commit everything to the database!
@survey.save
```

The will build a survey with the following node structure.

![alt tag](https://raw.githubusercontent.com/butchmarshall/active_record_survey/master/bin/Example_1.png)

### Answer Types

A number of different answer types are implemented by default.

 - [Default (a.k.a radio)](#answer_default)
 - [Boolean (a.k.a. checkbox)](#answer_checkbox)
 - [Rank](#answer_rank)
 - [Scale](#answer_scale)
 - [Text](#answer_text)

#### Default <a id="answer_default"></a>

```ruby
ActiveRecordSurvey::Node::Answer
```

#### Boolean <a id="answer_checkbox"></a>

```ruby
ActiveRecordSurvey::Node::Answer::Boolean
```

#### Rank <a id="answer_rank"></a>

```ruby
ActiveRecordSurvey::Node::Answer::Rank
```

#### Scale <a id="answer_scale"></a>

```ruby
ActiveRecordSurvey::Node::Answer::Scale
```

#### Text <a id="answer_text"></a>

```ruby
ActiveRecordSurvey::Node::Answer::Text
```

### Answer Validation



## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/butchmarshall/active_record_survey.

## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

