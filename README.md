
===============================================================================
CONNECT Iterative Data Analysis and Visualization Toolkit – User Guide
===============================================================================

# plot_iterations.py : Tool for analyzing the iterative process of CONNECT
This repository contains a tool for analyzing the iterative process of CONNECT (COsmological Neural Network Emulator of CLASS using TensorFlow - https://github.com/AarhusCosmology/connect_public), both with and without the use of a newly introduced feature: likelihood-filtering.

# Overview
--------
This program is a complete toolkit for analyzing and visualizing training data 
generated by the CONNECT iterative process. It automatically loads data from a 
specified project folder (which contains iteration subdirectories), processes the data,
and then produces multiple types of outputs:

  1. **Univariate Analysis:**  
     - Generates histograms, box plots, and summary statistics for likelihood values 
       and Δχ² (delta chi-squared) values. Only if enabled or the likelihood-filter was 
     - Uses the Analyze_likelihoods module.

  2. **Iteration Plots:**  
     - Creates detailed two-panel scatter plots that show both the new points generated 
       in an iteration (upper panel) and the accumulated state of the training data 
       up to that iteration (lower panel).  
     - Uses the PlotIterations module.

  3. **Triangle (Corner) Plot:**  
     - Produces a pairplot (corner plot) of the final iteration’s snapshot, contrasting 
       accepted vs. discarded points across multiple parameters.  
     - Uses the TrianglePlot module.

This guide explains how to run the entire program via the command line, the meaning and 
format of each option, and how to customize nearly every aspect of the analysis and visualization.

# Prerequisites and Installation
-------------------------------
Before running the program, ensure that you have the following Python libraries 
installed:

  - numpy
  - pandas
  - scipy
  - matplotlib
  - seaborn
  - tabulate

To check whether these packages are installed, run in a Unix shell:

    pip show numpy pandas scipy matplotlib seaborn tabulate

If any package is missing, install it using pip or conda. For example:

    pip install numpy pandas scipy matplotlib seaborn tabulate
    
Also ensure you have LaTeX installed on your system:
    which latex

If not installed, you can install texlive or other LaTeX distributions.:
    https://www.tug.org/texlive/

# Get started: Running the Program
-------------------
To get help on the program’s options, run:

    python plot_iterations.py --help

The program is invoked via the command-line interface. At a minimum, you must supply the 
path to your CONNECT project folder using the `--project_path` argument. Optionally, you can 
specify an output directory via `--output_path` (if not provided, a default subfolder within 
the project folder is used (e.g., `iteration_analysis`).

### Minimal Command Example
    python plot_iterations.py --project_path "/path/to/connect/data/lcdm_example"

This command loads the training data from the specified folder and then runs all analysis 
and plotting routines using default options. The above command will attempt to create the triangle plot,
with every single parameter, which can be very memory intensive, so it might terminate on a normal laptop.
It also create the two-panel scatter plot with default cosmological pairs (H0, omega_b).


### Full Command Example

    python plot_iterations.py --project_path "/path/to/connect/data/lcdm_example" \
                     --output_path "/path/to/output" \
                        --run_univariate_analysis "True" \                      
                        --run_triangle_plot "True" \
                        --run_plot_iterations "True" \
                        --triangle_plot_cosmoparams "['H0', 'omega_b', 'tau_reio']" \
                        --plot_iter_cosmoparams "[('H0', 'omega_b'), ('H0', 'tau_reio')]" \
                        --latex_labels "{'H0': r'$H_0$', 'omega_b': r'$\\Omega_b$', 'tau_reio': r'$\\tau_{reio}$'}" \ 
                        --dataloader_args "{'save_pickle_data': True, 'pickle_path': '/path/to/pickle_file'}" \
                        --analyze_lkl_args "{'x_range': [0, 5000], 'y_range': [0, 500], 'include_inset': True, 'save_formats': ['png', 'pdf']}" \
                        --plot_iter_args "{'x_range': [0, 100], 'y_range': [0, 100], 'combine_iteration_0_and_1': False, 'plot_contours': True, 'sigma': 15}" \
                        --triangleplot_args "{'iterations_to_plot': [0, 1, 2, 3, 4, 5], 'colormap': 'tab10', 'plot_contours': True, 'sigma': 12}"

All arguments except project_path are optional. But run_univariate_analysis, run_triangle_plot, run_plot_iterations 
are RECOMMENDED to be set to True, if you want to run the respective analysis and plotting routines.
And set to False, if you want to skip them.

The triangle_plot_cosmoparams and plot_iter_cosmoparams arguments are RECOMMENDED to specify the parameters to be plotted in the triangle plot and iteration plots, respectively.
They can also be set to "all" to include all parameters, but be aware that this can be memory-intensive for the triangle plot and excessive for the two-panel two parameter iteration plot, as it produces a plot for every pair of parameters.

Note: latex_labels, dataloader_args, analyze_lkl_args, plot_iter_args, triangleplot_args are optional arguments. They are only for customization of the output plots and analysis, if the default settings are not satisfactory. Such as default calculated axis ranges, default data categories plotted, default color maps, etc.
                                                    
This command runs the full analysis and visualization pipeline with customized options.
For all available custmization options for the `dataloader_args`, `analyze_lkl_args`, `plot_iter_args` and `triangleplot_args` dictionaries, please read the documentation in the code and try running the program with the `--help` flag.