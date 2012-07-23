task default: "build"


def dehamlize src
  dest = src.sub(/haml$/, "html").sub(/_haml/, "_layouts")
  puts "#{src} -> #{dest}"
  system "haml -f html5 #{src} #{dest}"
end

def remove src
  dest = src.sub(/haml$/, "html").sub(/_haml/, "_layouts")
  puts "rm #{dest}"
  system "rm #{dest}"
end

task :haml_watch do
  require 'listen'

  Rake::Task["haml_build"].invoke

  Listen.to("_haml/", filter: /\.haml$/) do |m, c, r|
    m.each { |f| dehamlize f }
    c.each { |f| dehamlize f }
    r.each { |f| remove f }
  end
end

task :haml_build do
  Dir.glob("_haml/*.haml") do |f|
    dehamlize f
  end
end

task :build do
  puts "Compiling HAML"
  Rake::Task["haml_build"].invoke
  puts "Compiling SASS"
  system "compass compile"
  puts "Building website"
  system "jekyll"
end

task :server do
  Rake::Task["build"].invoke
  system "foreman start"
end
