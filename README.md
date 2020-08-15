# multiplot

This package compiles a multiplot graph comprising multiple sub plots.

# Installation and usage

**Note**: For Windows (and probably MAC) users we recommend to use the latest snapshot containing gnuplot version 5.2.6 instead of 5.2.4. The snapshot for Windows can be downloaded here: http://sourceforge.net/projects/gretl/files/snapshots/gretl_install-64.exe/download

Get the package from the gretl package server and install it:
```
pkg install multiplot
```
Here is a sample script on how to use it (see also: https://raw.githubusercontent.com/atecon/multiplot/master/src/multiplot_sample.inp):

```
clear
set verbose off
open denmark -q

include multiplot.gfn

# Fine tune the graph if wished by setting a bundle
bundle options = defbundle("FONT_SIZE", 10, "PLOT_HEIGHT", 700)

# Call various gretl plots and store each one as a gnuplot-file (mimetype "*.gp")
gnuplot IBO --output=tf1.gp --time-series --with-lines { set title "foo" ; set yrange[0:0.5]; \
  set ylabel "ylabel"; }
gnuplot IDE --output=tf2.gp --time-series --with-lines { set title "foo" ; set xlabel "xlabel"; }
qqplot IBO --output=tf3.gp
freq IDE --plot=tf4.gp


# Multiplot with 4 rows + png output
# Pass the full path to each of the gp-files supposed to be added to the multiplot
string Gin = multiplot(defarray("tf1.gp","tf2.gp","tf3.gp","tf4.gp"),\
  "foo.png", 4, , options)

# Automatic grid determination + pdf output
Gin = multiplot(defarray("tf1.gp","tf2.gp","tf3.gp","tf4.gp"),\
  "my.pdf", , , options)
```

The resulting figure will look (similar) to this one:

![sample](https://github.com/atecon/multiplot/raw/master/screenshot.png)


# PUBLIC FUNCTION

Function:       *multiplot(const strings files_input, const string file_output,
                int n_rows, int n_cols, bundle options)*
Arguments:
- ```files_input```:    strings, Array of filenames each referring to a gnuplot file
- ```file_output```:    string, Full output file path for storing compiled plot (optional, default '0')
- ```n_rows```:         int, Number of rows of the multiplot grid; if zero determined (optional) by the number of columns chosen.
- ```n_cols```:         int, Number of columns of the multiplot grid (optional, default '0'); if zero determined by the number of rows chosen.
- ```options```:        bundle, Parameters for fine-tuning the plot (optional, see below for default values)

Return: *String variable including the gnuplot command. Plots are stored if ```file_output``` is specified.*


## Optional parameters:

The user can pass an optional bundle (```options```) as the fifth argument for fine-tuning the multiplot. The following parameters are currently supported and their default values are:

- ```PLOT_WIDTH```: default: 800 (pixels)
- ```PLOT_HEIGHT```: 600 (pixels)
- ```FONT_SIZE```: 12

# Changelog:
- v0.2, August 2020:
    + Bug fix: "display" mode did not work as no terminal was set in this case
    + Bug fix: Missing argument for sprintf() function call in case no
      supported terminal was passed.
- v0.11, June 2020:
	+ Add note to use gnuplot 5.2.6 (available in latest snapshot) due to some bug on Windows systems.
- v0.1, June 2020:
	+ initial release

