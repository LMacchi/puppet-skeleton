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

desc 'Run unit tests'
task :unit do
  Rake::Task[:spec_prep].invoke
  Rake::Task[:spec].invoke
end

desc 'Run lint test'
task :lint do
  Rake::Task[:lint].invoke
  Rake::Task[:metadata_lint].invoke
end

desc 'Run Puppet validate'
task :validate do
  Rake::Task[:validate].invoke
end

desc 'Create fixtures.yml from Puppetfile'
task :fixtures do
  sh ".scripts/fixtures_generate.rb -p ." 
end

desc 'Run integration test'
task :integ do
  Rake::Task[:integration].invoke
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
