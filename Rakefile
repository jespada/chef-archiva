#!/usr/bin/env rake

require 'bundler/setup'
require 'rspec/core/rake_task'
require 'rubocop/rake_task'
require 'foodcritic'
require 'kitchen'

namespace :style do
  desc 'Run Ruby style checks'
  RuboCop::RakeTask.new(:ruby)

  desc 'Run Chef style checks'
  FoodCritic::Rake::LintTask.new(:chef)
end

desc 'Run all style checks'
task style: ['style:chef', 'style:ruby']

desc 'Run ChefSpec unit tests'
RSpec::Core::RakeTask.new(:unit) do |t|
  t.rspec_opts = '--color --format progress'
end

# Integration tests. Kitchen.ci
desc 'Run Test Kitchen with Vagrant'
task :vagrant do
  Kitchen.logger = Kitchen.default_file_logger
  Kitchen::Config.new.instances.each do |instance|
    instance.test(:always)
  end
end

# Default
task default: %w(style unit vagrant)
