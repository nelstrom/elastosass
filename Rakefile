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
!#{var}_size                = #{values[var]["size"]     || values["defaults"]["size"]}
!#{var}_lineheights         = #{values[var]["lines"]    || values["defaults"]["lines"]}
!#{var}_margin_top_ratio    = #{values[var]["m_top"]    || values["defaults"]["m_top"]}
!#{var}_margins             = #{values[var]["margins"]  || values["defaults"]["margins"]}
!#{var}_padding_top_ratio   = #{values[var]["p_top"]    || values["defaults"]["p_top"]}
!#{var}_paddings            = #{values[var]["paddings"] || values["defaults"]["paddings"]}
!#{var}_border_top_ratio    = #{values[var]["b_top"]    || values["defaults"]["b_top"]}
!#{var}_border_bottom_ratio = #{values[var]["b_bot"]    || values["defaults"]["b_bot"]}

END
end

def definitions(var="h1")
  template = <<END

//  #{var}
//**************************************
#{var}
  :font-size = !#{var}_size
  :line-height = !#{var}_lineheight * !#{var}_lineheights
  :margin-top = !#{var}_margin_top
  :margin-bottom = !#{var}_margin_bottom
  :padding-top = !#{var}_padding_top
  :padding-bottom = !#{var}_padding_bottom
  :border
    :bottom
      :color #999
      :style solid
      :width = !#{var}_border_bottom_size
    :top
      :color #999
      :style solid
      :width = !#{var}_border_top_size
END
end

def calculations(var="h1")
    template = <<END

//  Calculations for #{var}
//**************************************
!#{var}_lineheight = !baseline / !#{var}_size
!#{var}_margin_bot_ratio = 1 - !#{var}_margin_top_ratio
!#{var}_padding_bot_ratio = 1 - !#{var}_padding_top_ratio

!#{var}_border_correction = !pixel_fudge / !#{var}_size
!#{var}_border_top_size = !#{var}_border_top_ratio * !#{var}_border_correction
!#{var}_border_bottom_size = !#{var}_border_bottom_ratio * !#{var}_border_correction

!#{var}_margin_top     = !#{var}_margin_top_ratio * !#{var}_margins * !#{var}_lineheight - !#{var}_border_top_size / 2
!#{var}_margin_bottom  = !#{var}_margin_bot_ratio * !#{var}_margins * !#{var}_lineheight - !#{var}_border_bottom_size / 2
!#{var}_padding_top    = !#{var}_padding_top_ratio * !#{var}_paddings * !#{var}_lineheight - !#{var}_border_top_size / 2
!#{var}_padding_bottom = !#{var}_padding_bot_ratio * !#{var}_paddings * !#{var}_lineheight - !#{var}_border_bottom_size / 2

END
  template
end