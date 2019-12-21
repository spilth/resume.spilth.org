require 'rdiscount'
require 'htmldoc'
require 'aws-sdk-s3'
require 'dotenv'
require 'dotenv/tasks'

task :default => :deploy

desc 'Generate HTML Version'
task :html do
  puts "Generating HTML from Markdown..."
  file = File.open("src/resume.md", "rb")
  contents = file.read
  file.close

  file = File.open("src/template.html", "rb")
  template = file.read
  file.close
  
  markdown = RDiscount.new(contents)
  resume_html = markdown.to_html
  html = template.gsub('CONTENT', resume_html)

  file = File.open("target/brian_kelly_resume.md", "w")
  file.write(contents)
  file.close

  file = File.open("target/index.html", "w")
  file.write(html)
  file.close

  puts "HTML Generated!"
end

desc 'Generate PDF version'
task :pdf => :html do
  puts "Gererating PDF from HTML..."
  
  pdf = PDF::HTMLDoc.new
  pdf.set_option :continuous, 1
  pdf.set_option :outfile, "target/brian_kelly_resume.pdf"
  pdf.set_option :size, "8.5x11in"
  pdf.set_option :left, "0.3in"
  pdf.set_option :right, "0.3in"
  pdf.set_option :bottom, "0.3in"
  pdf.set_option :linkstyle, "underline"
  pdf.set_option :linkcolor, "000099"
  pdf.set_option :toclevels, 0
  pdf.set_option :bodyfont, "helvetica"
  pdf.header "..."
  pdf.footer "dt/"

  pdf << "target/index.html"
  pdf.generate
  puts "PDF Generated!"
end

desc 'Deploy to Netlify'
task :deploy => [:pdf, :dotenv] do
  puts "Uploading files..."

  system 'netlify deploy --prod --dir target'
end

