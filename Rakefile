# Credit:
# Based on https://gist.github.com/417767

def config
  # in this Rakefile we can allow that
  $config ||= Hash.new
end

# main TeX file without extension
config['main'] = 'pbordjukov'

# TeX command to invoke: xelatex, pdflatex, etc
config['latex'] = 'pdflatex'

task :default => [ :pdf ]

desc 'Open the output PDF file'
task :view do
  invoke 'xdg-open', "#{config['main']}.pdf"
end

desc 'Build PDF'
task :pdf => [ :info, :clean ] do
  latex '-halt-on-error', "#{config['main']}.tex"
end

namespace :pdf do
  desc 'Build PDF in Draft Mode'
  task :draft do
    latex '-draftmode', '-halt-on-error', "#{config['main']}.tex"
  end
end

desc 'Cleanup'
task :clean do
  cleaner = proc { |path| rm_f(path) }
  [ 'aux', 'bbl', 'blg', 'pdf', 'toc', 'nav', 'log', 'out' ].each do |res|
    Dir.glob("*.#{res}", &cleaner)
  end
end

desc 'Print configuration'
task :info do
  puts "Configuration: #{config.inspect}"

  has_latex = latex '--version' rescue false
  raise 'LaTeX is not installed' unless has_latex
end

def invoke(command, *arguments)
  args = arguments.flatten.map { |arg| "'#{arg}'" }.join(' ')
  sh "#{command} #{args}"
end

def latex(*args)
  raise unless config['latex']
  invoke(config['latex'], args)
end
