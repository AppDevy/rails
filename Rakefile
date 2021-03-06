require 'rake'

env = %(PKG_BUILD="#{ENV['PKG_BUILD']}") if ENV['PKG_BUILD']

PROJECTS = %w(activesupport railties actionpack actionmailer activeresource activerecord)

Dir["#{File.dirname(__FILE__)}/*/lib/*/version.rb"].each do |version_path|
  require version_path
end

desc 'Run all tests by default'
task :default => :test

%w(test rdoc pgem package release gem).each do |task_name|
  desc "Run #{task_name} task for all projects"
  task task_name do
    passed = true
    PROJECTS.each do |project|
      system %(cd #{project} && #{env} #{$0} #{task_name})
      passed &&= $?.success?
    end
    exit! passed
  end
end

if RAKEVERSION == '0.8.0'
  require 'rake/rdoctask'

  # In order to generate the API please install Ruby 1.8.7 and rake 0.8.0. Then:
  #
  #     rake _0.8.0_ task_name
  #
  # Reason is the Jamis template used for the API needs RDoc 1.x. Recent rake libs
  # do not provide rake/rdoctask, and RDoc 1.x provides no alternative task. This
  # is easy to setup with a Ruby version manager.
  desc "Generate documentation for the Rails framework"
  Rake::RDocTask.new do |rdoc|
    rdoc.rdoc_dir = 'doc/rdoc'
    rdoc.title    = "Ruby on Rails Documentation"
    rdoc.main     = "railties/README"

    rdoc.options << '--line-numbers' << '--inline-source'
    rdoc.options << '-A cattr_accessor=object'
    rdoc.options << '--charset' << 'utf-8'
    rdoc.options << '--main' << 'railties/README'

    rdoc.template = ENV['template'] ? "#{ENV['template']}.rb" : './doc/template/horo'

    rdoc.rdoc_files.include('railties/CHANGELOG')
    rdoc.rdoc_files.include('railties/MIT-LICENSE')
    rdoc.rdoc_files.include('railties/README')
    rdoc.rdoc_files.include('railties/lib/{*.rb,commands/*.rb,rails/*.rb,rails_generator/*.rb}')

    rdoc.rdoc_files.include('activerecord/README')
    rdoc.rdoc_files.include('activerecord/CHANGELOG')
    rdoc.rdoc_files.include('activerecord/lib/active_record/**/*.rb')
    rdoc.rdoc_files.exclude('activerecord/lib/active_record/vendor/*')

    rdoc.rdoc_files.include('activeresource/README')
    rdoc.rdoc_files.include('activeresource/CHANGELOG')
    rdoc.rdoc_files.include('activeresource/lib/active_resource.rb')
    rdoc.rdoc_files.include('activeresource/lib/active_resource/*')

    rdoc.rdoc_files.include('actionpack/README')
    rdoc.rdoc_files.include('actionpack/CHANGELOG')
    rdoc.rdoc_files.include('actionpack/lib/action_controller/**/*.rb')
    rdoc.rdoc_files.include('actionpack/lib/action_view/**/*.rb')
    rdoc.rdoc_files.exclude('actionpack/lib/action_controller/vendor/*')

    rdoc.rdoc_files.include('actionmailer/README')
    rdoc.rdoc_files.include('actionmailer/CHANGELOG')
    rdoc.rdoc_files.include('actionmailer/lib/action_mailer/base.rb')
    rdoc.rdoc_files.exclude('actionmailer/lib/action_mailer/vendor/*')

    rdoc.rdoc_files.include('activesupport/README')
    rdoc.rdoc_files.include('activesupport/CHANGELOG')
    rdoc.rdoc_files.include('activesupport/lib/active_support/**/*.rb')
    rdoc.rdoc_files.exclude('activesupport/lib/active_support/vendor/*')
  end

  # Enhance rdoc task to copy referenced images also
  task :rdoc do
    FileUtils.mkdir_p "doc/rdoc/files/examples/"
    FileUtils.copy "activerecord/examples/associations.png", "doc/rdoc/files/examples/associations.png"
  end
end
