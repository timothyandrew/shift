require 'guard/guard'

module ::Guard
  class Shift < ::Guard::Guard
    def initialize(watchers = [], options = {})
      super
      @working_path = ARGV.include?("-w") ? ARGV[ARGV.index { |x| x == "-w" } + 1] : Dir.pwd

      puts "Processing #{@working_path}"
      run_on_change Dir.new(@working_path).find_all { |file| file =~ %r{.*torrent} }
    end

    def run_on_change(paths)
      paths.each do |path|
        full_path = File.join(@working_path, path)
        if system "scp \"#{full_path}\" linode:/srv/http/Torrents"
          Notifier.notify "Uploaded #{path} to linode."
          puts "Uploaded #{path} to linode."
          File.delete full_path
        else
          Notifier.notify "Failed on #{path}."
          puts "Failed on #{path}."
        end
      end
    end
  end
end

guard 'shift' do
  notification :growl, :sticky => true
  watch(%r{.*torrent})
end