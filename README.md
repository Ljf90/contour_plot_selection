# contour_plot_selection
A useful set of plotting functions to plot a contour plot for a selection

#### Contour cut plot

```python
cut_plot(xx, yy, exx, eyy, xycuts, xylogic, name, cs, xlabel, ylabel,
             savename, limits, lvls, use_limits=True, return_frame=False,
             plot_outliers=True, plot_rejected=False, plot_accepted=False,
             reject_from_contour=None, lw=1.5, compare=None, no_conts=False,
             custom_fig_size=None, no_legend=False, no_colourbar=False):
```

     Creates a contour plot with various options

    :param xx: array of float, x points

    :param yy: array of floats, y points

    :param exx: array of float, uncertainty in x points

    :param eyy: array of float, uncertainty in y points

    :param xycuts: list of cuts in the form:

                   xycuts = [cuts1, cuts2, ..., cutsn]

                   where cuts1 = [cut1, cut2, ..., cutn]

                   where cut1 = string logic statement

            if one wants a cut of x > 5 cut1 would be as follows:

                   cut1 = 'x > 5'

                   one may also use y and x as variables i.e.:

                   cut1 = 'y > 5*x'

            The logic is as follows:

                   if xycuts = [[cut1a, cut1b, cut1c], [cut2a, cut2b]]

                   the cuts produced are as follows:

                   cut1 = cut1a & cut1b & cut1c
                   cut2 = cut2a & cut2b

                   currently and is the only combination available, thus or
                   statements should be treated as separate cuts

            Thus an example xycuts could be:

                   [['x > 5', 'y > 5*x'], ['y > 2', 'y < 4']

    :param xylogic: currently not used (will be a list of '&' and '|' statements
                    in a similar configuration to xycuts such that one can
                    switch between "and" and "or" logic

                    default value should be [['&', '&'], ['&']]
                    i.e. must be the same shape as xycuts

    :param name: list of strings, name of each cut (for legend and stats)
                 must be equal to the length of xycuts

    :param cs: list of strings, line colour for each cut (for plotting)
               must be equal to the length of xycuts

    :param xlabel: string, label for the x axis
                   if xlabel == 'V-J' I assume this is a proxy for spectral
                   type and thus twin the axis to show spectral type

    :param ylabel: string, label for the y axis

    :param savename: string, location and filename (without extension) for the
                     created plot

    :param limits: limits of the plot in x and y in the form:
                        [ [x lower limit, x upper limit],
                          [y lower limit, y upper limit]]

    :param lvls: list of numbers, contour levels, this defines the levels
                 and number of levels
                 i.e. lvls = [2, 5, 10, 20, 50]

    :param use_limits: bool, if True uses "limits" parameter to add an extra
                       mask to the plot i.e. do not plot any points outside
                       the area that will be plot finally (saves on computation
                       in the case of a large number of points)

    :param return_frame: bool, if True does not save or show the graph,
                         simply returns the matplotlib.axes for further use

    :param plot_outliers: bool, if True plots those points which do not fall
                          into the contours (i.e. below the density required
                          by "lvls" these points will be plotted in
                          black. This saves having to plot all the points
                          under the contours (smaller plot save size, and easier
                          to load)

    :param plot_rejected: bool, if True plots any points rejected (i.e. False)
                          by the cuts defined by xycuts

    :param plot_accepted: bool, if True plots any points accepted (i.e. True)
                          by the cuts defined by xycuts

    :param reject_from_contour: array of booleans, an additional mask of
                                parameters to not use in the contour plot
                                (nans and infinites already removed)

    :param lw: float, standard matplotlib linewidth keyword argument for the
               widths of the cut lines plotted

    :param compare: list of variables or None, if None then do not plot
                    if a list then list must be in the form
                    compare = [xs, ys, ss, ls, bs]

                    where xs, ys are arrays of floats

                          ss, ls are array of strings, spt and luminosity class
                          respectively

                          bs is an array of booleans, to flag binary

                    (see plot_comparison function for more information)

    :param no_conts: bool, if True the outliers, rejected and accepted points
                     will NOT be plotted as contours i.e. all points will be
                     plotted separately

    :param custom_fig_size: tuple or None, if tuple should be the size in
                            inches of the image in (x, y)
                            i.e. for use in fig.set_size_inches()

    :param no_legend: bool, if True do not plot the legend

    :param no_colourbar: bool, if True do not plot a colorbar

    :return:

##### Example code

```python
# -------------------------------------------------------------------------
# Example use of cut plot
# -------------------------------------------------------------------------
vj_names = ['$V-J > 4$ and $V > 15$ cut', r'$V-J>2.7$ cut']
vj_cuts = [['x > 4', 'y > 15'], ['x > 2.7']]
vj_logic = [['&']]
vj_colours = ['r', 'g']
vj_limits = [[0, 8], [25, 5]]
vj_labels = ['$V-J$', '$V$']
vj_compare = True
# -------------------------------------------------------------------------
# make some fake data
# -------------------------------------------------------------------------
v = np.random.normal(15, 2, size=10000)
vj = np.random.normal(4.0, 1.0, size=10000)
ev, evj = 0.1*np.sqrt(v), 0.1*np.sqrt(vj)
clevels = [2, 5, 10, 50]
vc = np.random.normal(15, 2, size=100)
vjc = np.random.normal(4.0, 1.0, size=100)
numspt = np.random.choice(range(-60, 30) + range(0, 10)*5, size=100)
lumspt = np.random.choice([-100, 0, 0, 0, 0, 0, 0, 100], size=100)
binaryflag = np.random.choice([True, False], size=100)
# -------------------------------------------------------------------------
# plot V-J vs V band
# -------------------------------------------------------------------------
print ('\n Plotting V-J vs V band...')
psave = None
cut_plot(vj, v, evj, ev, vj_cuts, vj_logic, vj_names,
     vj_colours, vj_labels[0], vj_labels[1], psave, vj_limits, clevels,
     compare=[vjc, vc, numspt, lumspt, binaryflag])
```
