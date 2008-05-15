require 'rubygems'
require 'rake'
require 'fileutils'
require 'yaml'

desc "Load yaml and do something with it"
task :yaml do
  d = YAML::load_file("./defaults.yml")
  require 'pp'
  pp d
end

desc "Generate a variables.sass file"
task :variables do
  path = "./src/stylesheets/variables.sass"
  FileUtils.mkdir_p(File.dirname(path))
  File.open(path, 'w+') do |f|
    f << variables_prelude
    types.each do |type|
      f << variables(type)
    end
  end
end

desc "Generate a calculate.sass file"
task :calculate do
  path = "./src/stylesheets/calculate.sass"
  FileUtils.mkdir_p(File.dirname(path))
  File.open(path, 'w+') do |f|
    f << calculations_prelude
    types.each do |type|
      f << calculations(type)
    end
  end
end

desc "Generate a definitions.sass file"
task :definitions do
  path = "./src/stylesheets/definitions.sass"
  FileUtils.mkdir_p(File.dirname(path))
  File.open(path, 'w+') do |f|
    f<< definitions_prelude
    types.each do |type|
      f << definitions(type)
    end
  end
end

desc "Generate variables.sass and calulate.sass"
task :all => [:variables, :calculate, :definitions]

def types
  @defaults = YAML::load_file("./defaults.yml")
  # TODO remove the "default" key/values
  @defaults.keys
  @defaults.keys.sort
end

def variables_prelude
  template = <<END
!baseline = 1.5em
!basesize = 1em
END
end

def definitions_prelude
  template = <<END
// This is where vertical rhythm is set for the supplied elements 

@import calculate.sass

END
end

def calculations_prelude
  template = <<END
//  This file is auto generated.
//  It is recommended that you leave it alone
//********************************************

// Some constants 
!pixel_fudge = 0.075

@import variables.sass
END
end

def variables(var="h1", values=@defaults)
  template = <<END
//  #{var}
//**************************************
!#{var}_size        = #{values[var]["size"]  || values["defaults"]["size"]}
!#{var}_lineheights = #{values[var]["lines"] || values["defaults"]["lines"]}
!#{var}_whitespace  = #{values[var]["ws"]    || values["defaults"]["ws"]}
!#{var}_top_space   = #{values[var]["ts"]    || values["defaults"]["ts"]}
!#{var}_top_margin  = #{values[var]["tm"]    || values["defaults"]["tm"]}
!#{var}_top_border  = #{values[var]["tb"]    || values["defaults"]["tb"]}
!#{var}_bot_margin  = #{values[var]["bm"]    || values["defaults"]["bm"]}
!#{var}_bot_border  = #{values[var]["bb"]    || values["defaults"]["bb"]}
END
end

def calculations(var="h1")
    template = <<END

//  Calculations for #{var}
//**************************************
!#{var}_baseline           = !baseline / !#{var}_size
!#{var}_bot_space          = 1 - !#{var}_top_space
!#{var}_top_padding        = 1 - !#{var}_top_margin
!#{var}_bot_padding        = 1 - !#{var}_top_padding

!#{var}_border_correction  = !pixel_fudge / !#{var}_size
!#{var}_border_top_size    = !#{var}_top_border * !#{var}_border_correction
!#{var}_border_bottom_size = !#{var}_bot_border * !#{var}_border_correction

!#{var}_top_whitespace     = !#{var}_whitespace * !#{var}_top_space * !#{var}_baseline
!#{var}_bot_whitespace     = !#{var}_whitespace * !#{var}_bot_space * !#{var}_baseline

!#{var}_margin_top         = !#{var}_top_margin * !#{var}_top_whitespace - !#{var}_border_top_size
!#{var}_margin_bottom      = !#{var}_bot_margin * !#{var}_bot_whitespace - !#{var}_border_bottom_size
!#{var}_padding_top        = !#{var}_top_padding * !#{var}_top_whitespace
!#{var}_padding_bottom     = !#{var}_bot_padding * !#{var}_bot_whitespace


END
  template
end

def definitions(var="h1")
  template = <<END

//  #{var}
//**************************************
#{var}
  :font-size = !#{var}_size
  :line-height = !#{var}_baseline * !#{var}_lineheights
  :margin-top = !#{var}_margin_top
  :border-top-width = !#{var}_border_top_size
  :padding-top = !#{var}_padding_top
  
  :padding-bottom = !#{var}_padding_bottom
  :border-bottom-width = !#{var}_border_bottom_size
  :margin-bottom = !#{var}_margin_bottom
  
  :border
    :bottom
      :color #999
      :style solid
    :top
      :color #999
      :style solid
END
end