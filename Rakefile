require 'puppetlabs_spec_helper/rake_tasks'
require 'puppet-lint/tasks/puppet-lint'
require 'metadata-json-lint/rake_task'

if RUBY_VERSION >= '1.9'
  require 'rubocop/rake_task'
  RuboCop::RakeTask.new
end

PuppetLint.configuration.send('disable_80chars')
PuppetLint.configuration.send('disable_autoloader_layout')
PuppetLint.configuration.relative = true
PuppetLint.configuration.ignore_paths = ['spec/**/*.pp', 'pkg/**/*.pp']

desc 'Validate manifests, templates, and ruby files'
task :validate do
  Dir['manifests/**/*.pp'].each do |manifest|
    sh "puppet parser validate --noop #{manifest}"
  end
  Dir['spec/**/*.rb', 'lib/**/*.rb'].each do |ruby_file|
    sh "ruby -c #{ruby_file}" unless ruby_file =~ %r{spec/fixtures}
  end
  Dir['templates/**/*.erb'].each do |template|
    sh "erb -P -x -T '-' #{template} | ruby -c"
  end
  Dir['templates/**/*.epp'].each do |template|
    sh "puppet epp validate #{manifest}"
  end
end

desc 'Run metadata_lint, lint, validate, and spec tests'
task :test do
  [:metadata_lint, :lint, :validate, :spec].each do |test|
    Rake::Task[test].invoke
  end
end

desc 'Create fixtures.yml from Puppetfile'
task :fixtures do
  puts "Warning: Overwriting .fixtures.yml"
  sh "#{Dir.pwd}/.scripts/fixture_generator.rb -p #{Dir.pwd} -d -f"
end

desc 'Install git hooks'
task :install_git_hooks do
  source = "#{Dir.pwd}/.git_hooks"
  target = './.git/hooks'
  git_hooks_available = Dir.entries(source)
  git_hooks_available.each do |hook|
    if (hook != '.' and hook != '..' and hook != 'README.md') then
      FileUtils.rm_rf  "#{target}/#{hook}"
      FileUtils::cp "#{source}/#{hook}", "#{target}/#{hook}"
      FileUtils::chmod 0755, "#{target}/#{hook}"
    end
  end
  FileUtils::touch '.git_hooks_installed'
end

# remove undesired rake tasks
task :build => []; Rake::Task[:build].clear
task :clean => []; Rake::Task[:clean].clear
task :coverage => []; Rake::Task[:coverage].clear
task :beaker => []; Rake::Task[:beaker].clear
task :beaker_nodes => []; Rake::Task[:beaker_nodes].clear
task 'beaker:sets' => []; Rake::Task['beaker:sets'].clear
task 'beaker:ssh' => []; Rake::Task['beaker:ssh'].clear
task :rubocop => []; Rake::Task[:rubocop].clear
task 'rubocop:auto_correct' => []; Rake::Task['rubocop:auto_correct'].clear
task 'check:dot_underscore' => []; Rake::Task['check:dot_underscore'].clear
task 'check:git_ignore' => []; Rake::Task['check:git_ignore'].clear
task 'check:symlinks' => []; Rake::Task['check:symlinks'].clear
task 'check:test_file' => []; Rake::Task['check:test_file'].clear
task :compute_dev_version  => []; Rake::Task[:compute_dev_version].clear

task :default => []; Rake::Task[:default].clear
task :default => :test
