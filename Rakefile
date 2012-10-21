require 'rake'
require 'erb'
require 'yaml'

# Load data from YAML file
data = YAML.load_file("resume.yaml")

# Create 'output' directory if it doesn't exist
#Dir.mkdir("output") unless File.exists?("output") && File.directory?("output")

file data['tex']['output'] => data['tex']['template'] do |t|
  template = ERB.new(File.read(t.prerequisites[0]), 0, ">")
  File.open(t.name, 'w') do |f|
    f.write(template.result(binding))
  end
end

# Generate the HTML output file
file data['html']['output'] => data['html']['template'] do |t|
  template = ERB.new(File.read(t.prerequisites[0]), 0, ">")
  File.open(t.name, 'w') do |f|
    f.write(template.result(binding))
  end
end

desc "Generates the HTML output"
task :html => data['html']['output']

desc "Generates the Latex output"
task :tex => data['tex']['output']

desc "Generate the PDF file from the Latex file"
task :pdf => [:tex] do
  puts "Generating PDF..."
end

desc "Cleans everything up"
task :clean do
  `rm -f *.tex *.log *.pdf  *.html`
end

desc "Publishes the HTML and PDF output to the Github pages"
task :gh_pages => [:html, :pdf] do
  #mv #{data[:tex][:pdf]} cv-`date +%Y-%m`.pdf
  Kernel.exec(<<-CMD)
    set -e
    git checkout gh-pages
    mv #{data[:html][:output]} index.html
    git add .
    git commit -m 'index page update'
    git checkout master
  CMD
end

task :server => [:html] do
  `python -m SimpleHTTPServer`
end


task :default => [:clean, :html, :tex]
