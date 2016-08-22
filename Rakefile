#!/usr/bin/env rake

require 'html/proofer'
require 'html/pipeline'
require 'pathname'
require 'fileutils'
require 'open-uri'

desc 'Clean up generated site'
task :clean do
  sh 'bundle exec jekyll clean'
end

task :test do
  sh 'bundle exec jekyll build --config _config.dev.yml'

  url_ignore = []
  url_ignore.push(/#/)

  url_swap = {
      # pretend any .md link is really a .html link
      /\.md/ => '.html'
  }

  begin
    # test your ./_site dir!
    HTML::Proofer.new('./_site', {
        :empty_alt_ignore => true,
        :url_ignore  => url_ignore,
        :url_swap => url_swap,
        :connecttimeout => 600,
        :typhoeus => { :ssl_verifypeer => false, :ssl_verifyhost => 0, :followlocation => true }
    }).run
  ensure
    FileUtils.rm_rf('tmp')
  end
end

desc 'Runs the tests!'
task default: :test
