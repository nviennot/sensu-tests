require 'rake'


desc "install bundler and other gems"
task :bundler_install do
  unless Gem.available? 'bundler'
    sh "gem install bundler --no-rdoc --no-ri --quiet"
  end
  sh "bundle install --quiet"
end

desc "run rspec tests"
task :spec => [:bundler_install] do
  # we are shelling out to rspec on purpose to avoid some bundler issues
  # that can happen with the ruby in /opt/sensu. Use RSpec::RakeTasks at your
  # own peril (or submit a patch if you make it work)
  sh "rspec -fd spec/spec_*.rb"
end

desc "run librarian-chef to pull down all cookbooks"
task :chef_prep do
  sh "cd chef && librarian-chef install"
end

desc "execute vagrant to build all machines and run tests"
task :vagrant => [:bundler_install, :chef_prep] do
  sh "./para-vagrant.sh"
end



task :usage do
  puts "There is no default task since this single Rakefile is meant to"
  puts "be run in two different contexts: "
  puts "   1) outside a VM to call vagrant and spin up boxes"
  puts "   2) inside a VM to run the test suite"
end

task :default => :usage