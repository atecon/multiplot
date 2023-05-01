# Introduction

This package compiles a multiplot graph comprising multiple subplots. The subplots can be produced with various plotting commands in gretl, which are all based on the gnuplot program.

Please ask questions and report bugs on the gretl mailing list if possible. Alternatively, create an issue ticket on the github repo. Source code and test scripts can also be found there: https://github.com/atecon/multiplot


# Public function

```
multiplot(const strings input_data, const string file_output, int n_rows, int n_cols, bundle options)
```

Main function for creating a multiplot.

## Parameters


- `input_data`: strings, Array of filenames each referring to a gnuplot file or alternatively array of gnuplot command text strings. One cannot mix filenames and buffers, though! In case you need to mix gnuplot input from files and from string buffers, simply use the native `readfile()` function to load file contents into a string variable.
- `file_output`: string, Full output file path for storing compiled plot (optional, default null implies "display")
- `n_rows`: int, Number of rows of the multiplot grid (optional; if 0 or omitted, it is determined by the number of columns chosen)
- `n_cols`: int, Number of columns of the multiplot grid (optional; if 0 or omitted, it is determined by the number of rows chosen)
- `options`: bundle, Parameters for fine-tuning the plot (optional, see below for default values)

## Returns

String variable containing the gnuplot commands. Plots are stored to disc if `file_output` is specified.


## Optional parameters

The user can pass an optional bundle (`options`) as the fifth argument; mainly for fine-tuning the multiplot. The following parameters are currently supported
and their default values are:

- `PLOT_WIDTH`: Width of the plot. Default: 800 (pixels)
- `PLOT_HEIGHT`: Height of the plot. Default: 600 (pixels)
- `FONT_SIZE`: Font size. Default: 10 pt
- `only_input_files`: Boolean switch which should be set to TRUE if argument input_data only contains filenames and no gnuplot command strings, otherwise FALSE. (Optional; if absent, an automatic detection is applied.)



# Changelog

* **v0.4 (May 2023)**
    * New minimum version is 2022d.
    * Fix bug with odd number of plots (for example 5) and supply the n_rows and n_cols option to the multiplot function with neither of the values equal to 1.
    * Internal improvements.

* **v0.3 (January 2023)**
    * Accept the direct input of gnuplot command file contents in the strings array instead of the names of temporary files.
    * Require gretl 2021a because of modern internal bundle syntax.
    * Various small other edits
    * Set new default font size to 10 pt
    * Add mechanism for checking validity of parameter values

* **v0.2 (August 2020)**
    * Bug fix: "display" mode did not work as no terminal was set in this case.
    * Bug fix: Missing argument for `sprintf()` function call in case no supported terminal was passed.
    * Switch from "png" to "pngcairo" terminal.

* **v0.11 (June 2020)**
    * Add note to use gnuplot 5.2.6 (available in latest snapshot) due to some bug on Windows systems.

* **v0.1 (June 2020)**
    * initial release
