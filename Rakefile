require 'fileutils'
require 'find'

SRC_DIR = '/home/pi/tdlmn/src/tiles/dem5a_png'
DST_DIR = 'docs/zxy'
COMMAND = 'node ../boxer/index.js'
SITE_ROOT = "https://optgeo.github.io/boxer-dem5a"
Z = 7
X = 113
Y = 50

desc 'produce tiles'
task :produce do
  Find.find(SRC_DIR) {|path|
    next unless /(\d*)\/(\d*)\/(\d*)\.png$/.match path
    (z, x, y) = [$1, $2, $3].map{|v| v.to_i}
    next unless z < Z || (x >> (z - Z) == X && 
      y >> (z - Z) == Y)
    sh "mkdir -p #{DST_DIR}/#{z}/#{x}"
    sh [
      "SUPPRESS_NO_CONFIG_WARNING=1",
      COMMAND,
      "< #{SRC_DIR}/#{z}/#{x}/#{y}.png",
      "> #{DST_DIR}/#{z}/#{x}/#{y}.png"
    ].join(' ')
  }
end

desc 'generate style.json'
task :style do
  sh [
    "SITE_ROOT=#{SITE_ROOT}",
    "parse-hocon hocon/style.conf > docs/style.json"
  ].join(" ")
  sh "gl-style-validate docs/style.json"
end

desc 'host the site locally'
task :host do
  sh "budo -d docs"
end

