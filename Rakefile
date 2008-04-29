require 'rubygems'
require 'rake'
require 'fileutils'

desc "Generate a calculate.sass file"
task :generate do
  path = "./src/stylesheets/test.sass"
  FileUtils.mkdir_p(File.dirname(path))
  File.open(path, 'w+') do |f|
    f << headings
  end
end

def headings
  content = template()
  content << template("h2")
  content << template("h3")
end

def template(var="h1")
    template = <<END
!#{var}_margin_top_ratio = 0.5
!#{var}_margin_bot_ratio = 1 - !#{var}_margin_top_ratio
!#{var}_margins = 1

!#{var}_padding_top_ratio = 0.5
!#{var}_padding_bot_ratio = 1 - !#{var}_padding_top_ratio
!#{var}_paddings = 1

!pixel_fudge = 0.075
!#{var}_pixel_fudge = !pixel_fudge / !#{var}_size
!#{var}_border_correction = 0.025em
!#{var}_border_correction = !pixel_fudge / !#{var}_size
!#{var}_border_top_ratio = 1
!#{var}_border_top_size = !#{var}_border_top_ratio * !#{var}_border_correction
!#{var}_border_bottom_ratio = 1
!#{var}_border_bottom_size = !#{var}_border_bottom_ratio * !#{var}_border_correction

!#{var}_whitespace = 1
!#{var}_margin_top_W = !#{var}_margin_top_ratio * !#{var}_whitespace
!#{var}_margin_bot_W = !#{var}_margin_bot_ratio * !#{var}_whitespace

!#{var}_margin_top     = !#{var}_margin_top_ratio * !#{var}_margins * !#{var}_lineheight - !#{var}_border_top_size
!#{var}_margin_bottom  = !#{var}_margin_bot_ratio * !#{var}_margins * !#{var}_lineheight - !#{var}_border_bottom_size
!#{var}_padding_top    = !#{var}_padding_top_ratio * !#{var}_paddings * !#{var}_lineheight
!#{var}_padding_bottom = !#{var}_padding_bot_ratio * !#{var}_paddings * !#{var}_lineheight
!#{var}_border_top     = 1
!#{var}_border_bottom  = 1
END
end