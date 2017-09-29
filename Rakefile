require 'date'

desc 'create a new draft post'
task :post do
  title = ENV['title']
  slug = "#{Date.today}-#{title.downcase.gsub(/[^\w]+/, '-')}"

  file = File.join(File.dirname(__FILE__), '_posts', slug + '.markdown')
  
  File.open(file, "w") do |f|
    f << <<-EOS.gsub(/^    /, '')
    ---
    layout: post
    title: #{title}
    date:   #{Time.now}
    categories:
    ---

    EOS
  end
end