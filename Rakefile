require 'yaml'
require 'fileutils'
require 'pp'

DOTFILES_HOME = ENV['HOME']
DOTFILES_ROOT = File.dirname(__FILE__)
CONFIG_PATH   = File.join(DOTFILES_ROOT, 'config.yml')
CONFIG        = YAML.load_file(CONFIG_PATH)

desc "Show current config"
task :config do
  CONFIG.each_pair do |name, files|
    puts "#{name}:"
    files.each { |f| puts " - #{f}" }
  end
end

desc "Install dot files"
task :dotfiles do
  files     = CONFIG['dotfiles']

  files.each do |name|
    path      = File.join(DOTFILES_ROOT, 'dotfiles', name)
    link_path = File.join(DOTFILES_HOME, ".#{name}")

    system "unlink #{link_path}" if File.exists?(link_path)
    system "ln -vsf #{path} #{link_path}"
  end
end

desc "Install bin files"
task :binfiles do
  files     = CONFIG['binfiles']

  FileUtils.mkdir_p("#{DOTFILES_HOME}/bin")

  files.each do |name|
    path      = File.join(DOTFILES_ROOT, 'bin', name)
    link_path = "#{DOTFILES_HOME}/bin/#{File.basename(name)}"

    system "unlink #{link_path}" if File.exists?(link_path)
    system "ln -vsf #{path} #{link_path}"
  end
end

desc "Install all settings"
task :install do
  Rake::Task['dotfiles'].invoke
  Rake::Task['binfiles'].invoke
end
