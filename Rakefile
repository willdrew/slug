require 'rake'

begin
  require 'jeweler'
  Jeweler::Tasks.new do |s|
    s.name = "slug"
    s.summary = %Q{Simple, straightforward slugs for your ActiveRecord models.}
    s.email = "ben.koski@gmail.com"
    s.homepage = "http://github.com/bkoski/slug"
    s.description = "Simple, straightforward slugs for your ActiveRecord models."
    s.add_dependency 'activerecord', '> 3.0.0'
    s.add_dependency 'activesupport', '> 3.0.0'
    s.authors = ["Ben Koski"]
  end
  Jeweler::GemcutterTasks.new
rescue LoadError
  puts 'Jeweler not available. In order to run jeweler, please run: bundle install'
end

require 'rdoc/task'
Rake::RDocTask.new do |rdoc|
  rdoc.rdoc_dir = 'rdoc'
  rdoc.title = 'slug'
  rdoc.options << '--line-numbers' << '--inline-source' << '--all'
  rdoc.rdoc_files.include('README*')
  rdoc.rdoc_files.include('lib/**/*.rb')
end

require 'rake/testtask'
Rake::TestTask.new(:test) do |t|
  t.libs << 'lib' << 'test'
  t.pattern = 'test/**/*_test.rb'
  t.verbose = false
end

begin
  require 'simplecov'
  SimpleCov.command_name 'test:units'
  SimpleCov.start do
    add_filter '/test/'
  end
  require_relative 'test/test_slug'

rescue LoadError
  puts 'simplecov is not available. In order to run simplecov, please run: bundle install'
end

task :default => :test
