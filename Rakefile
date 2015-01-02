require "rubygems"
require "bundler/setup"
require "stringex"

deploy_dir = "_site"
post_dir   = "_posts"
new_post_ext = "markdown"
remote = "origin"  #remote repo name
source_branch = "master" 
site_branch = "gh-pages 



# task :post, :title do |t, args|
#   args.with_defaults(:title => 'new-post')
#   filename = "#{post_dir}/#{Time.now.strftime('%Y-%m-%d')}-#{args.title.to_url}.#{new_post_ext}"
#   if(File.exist?(filename))
#     puts filename + "already exists"
#     abort("rake aborted!")
#   end
#   open(filename, 'w') do |post|
#     post.puts "---"
#     post.puts "layout: post"
#     post.puts "title: \"#{args.title.gsub(/&/,'&amp;')}\""
#     post.puts "date: #{Time.now.strftime('%Y-%m-%d %H:%M')}"
#     post.puts "comments: true"
#     post.puts "categories: "
#     post.puts "---"
#   end
#  end

task :deploy do |t, args|
  puts "stage modified files"
  system("git add -A")
  message = "deploy jekyll website"
  system("git commit -m \"#{message}\"")

  puts "update remote source branch"
  system("git push #{remote} #{source_branch}")

  puts "build jekyll site"
  system("jekyll build")
  puts "update remote gh-pages branch(origin/gh-pages)"
  system("cd #{deploy_dir} && git commit -m \"#{message}\"") #do stage and commit together
  system("cd #{deploy_dir} & git push #{remote} #{site_branch}") 
end

  
task :post, :title do |t, args|
  args.with_defaults(:title => 'new-post')
  filename = "#{post_dir}/#{Time.now.strftime('%Y-%m-%d')}-#{args.title.to_url}.#{new_post_ext}"
  if(File.exist?(filename))
    puts filename + "already exists"
    abort("rake aborted!")
  end
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{args.title.gsub(/&/,'&amp;')}\""
    post.puts "date: #{Time.now.strftime('%Y-%m-%d %H:%M')}"
    post.puts "comments: true"
    post.puts "categories: "
    post.puts "---"
  end
 end




