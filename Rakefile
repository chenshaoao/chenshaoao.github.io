require 'time'

# Usage: rake post title="A Title" [date="2014-04-14"]
desc "Create a new post" 
task :post do
  unless FileTest.directory?('./_posts')
    abort("rake aborted: '_posts' directory not found.")
  end
  title = ENV["title"] || "new-post"
  slug = title.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '') 
  begin
    date = (ENV['date'] ? Time.parse(ENV['date']) : Time.now)
               .strftime('%Y-%m-%d')
  rescue Exception => e
    puts "Error: date format must be YYYY-MM-DD!" 
    exit -1
  end
  filename = File.join('.', '_posts', "#{date}-#{slug}.md") 
  if File.exist?(filename)
    abort("rake aborted: #{filename} already exists.") 
  end

  puts "Creating new post: #{filename}" 
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{title.gsub(/-/,' ')}\"" 
    post.puts "date: #{date}"
    post.puts "categories:"
    post.puts "---"
  end 
end

# Usage: rake draft title="A Title"
desc "Create a new draft" 
task :draft do
  unless FileTest.directory?('./_drafts')
    abort("rake aborted: '_drafts' directory not found.")
  end
  title = ENV["title"] || "new-post"
  slug = title.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '') 
  filename = File.join('.', '_drafts', "#{slug}.md")
  if File.exist?(filename)
    abort("rake aborted: #{filename} already exists.") 
  end

  puts "Creating new draft: #{filename}" 
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{title.gsub(/-/,' ')}\"" 
    post.puts "date:"
    post.puts "categories:"
    post.puts "---"
  end 
end

# Usage: rake push
desc "Push to github" 
task :push do
  puts   "Pushing to 'master' branch:"
  system "git push origin master"
  puts   "'master' branch updated."
  puts

  puts   "Building site...."
  system "bundle exec jekyll build"
  puts   "Site updated."
  puts

  cd '_site' do
    puts "Pushing to 'chenshaoao.github.io master' branch:"
    system "git add -A"
    system "git commit -m 'update at #{Time.now.utc}'" 
    system "git push origin master"
    puts "'chenshaoao.github.io master' branch updated."
  end 
end

