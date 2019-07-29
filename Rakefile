require 'yaml'

SIDENAV_INDEX_DEFAULT_TITLE = 'Overview'
SIDENAV_FILE_BLACKLIST = [
  './vendor/**/*.md', 
  './legal/**/*.md', 
  './api/**/*.md', 
  './connections/**/*.md', 
  './*.md', 
  './_*/**/*.md'
]


namespace :nav do
  desc 'Updates _data/sidenav.yml based on the current available docs'
  task :update do
    p 'Updating _data/sidenav.yml based on current docs...'

    docs = FileList.new('./**/*.md').exclude(SIDENAV_FILE_BLACKLIST)
    nav = []
    sections = Hash.new()

    docs.map do |file_list|
      paths = file_list.to_s
                      .split('/')
                      .reject { |p| p == '.'}
      

      if paths.size == 0 || paths.size > 3
        # Not a valid path or some deep-nested directory structure we don't support
        next
      end
    
      root = paths[0]
      k = root
      sections[k] ||= { 'section_title' => root.capitalize, 'section' => [{ 'path' => "/#{root}", 'title' => SIDENAV_INDEX_DEFAULT_TITLE}] }

      if paths.size <= 3
        path = nil
        path_title = nil

        next if paths.last == "index.md"
        
        path = paths[0..paths.size].join("/").gsub(".md", "")

        begin
          f = YAML.load_file("./#{path}.md")
          path_title = f["title"]
        rescue
          path_title = path.split("/").last.gsub("-", " ").capitalize
        end

        sections[k]['section'] << { 'section' => [{ 'path' => "/#{path}", 'title' => path_title }]}
      end
    end

    nav = { 'sections' => sections.values }

    File.open("./_data/sidenav.yml","w") do |file|
      file.write nav.to_yaml({ indention: 4, separator: '' })
    end 
  end
end

namespace :catalog do
  desc 'Updates _data/catalog.yml based on the current catalog available in the Platform API'
  task :update do
    p 'Updating _data/catalog.yml based on current Segment Catalog...'
  end
end
