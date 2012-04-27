require 'guard/guard'

module ::Guard
  class Shift < ::Guard::Guard
    def initialize(watchers = [], options = {})
      super
      @working_path = ARGV.include?("-w") ? ARGV[ARGV.index { |x| x == "-w" } + 1] : Dir.pwd
    end

    def run_on_change(paths)
      paths.each do |path|
        full_path = File.join(@working_path, path)
        if system "scp \"#{full_path}\" linode:/srv/http/Torrents"
          puts "Uploaded #{path} to linode."
          File.delete path
        else
          puts "Failed on #{path}."
        end
      end
    end
  end
end

guard 'shift' do
  watch(%r{.*torrent})
end