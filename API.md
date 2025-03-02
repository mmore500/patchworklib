## Basic Functions

- ### **`load_ggplot(ggplot=None, figsize=None)`**

  Convert a plotnine plot object to a `patchworklib.Bricks` object.

  #### Parameters

  - `ggplot`: plotnine.ggplot.ggplot  
  - `figsize`: tuple (float, float)   
      Figure size. If it is not specified, the generated Bricks object will keep the 
      original figure size of the given ggplot object.

  #### Returns

  `patchworklib.Bricks` object
  
- ### **`overwrite_axisgrid()`**

  Overwrite the `__init__` methods in `seaborn.axisgrid.xxGrid` classes.
  The function changes the figure object given in the `__init__` functions of the 
  axisgrid class objects, which is used for drawing plots, to `_basefigure` 
  in the patchworklib. If you want to load the plots generated based on 
  `seaborn.axisgrid.xxGrid` objects as `patchworklib.Brick(s)` object using 
  `load_seaborngrid` function, you should execute this function in advance.

  #### Returns

  `None`

- ### **`patched_axisgrid()`**

  [Context manager](https://docs.python.org/3/reference/compound_stmts.html#with)/[decorator](https://docs.python.org/3/glossary.html#term-decorator)
  interface for `overwrite_axisgrid` patching that reverts changes when leaving
  `with`/function scope.

  #### Returns

  Context manager (i.e., `with patched_axisgrid():`) or decorator (i.e.,
 `@patched_axisgrid()`) that temporarily patches seaborn for patchworklib
  compatibility.

- ### **`load_seabornobj(g, label=None, labels=None, figsize=(3, 3))`**

  Load a seaborn plot generated based on `seaborn._core.plot.Plotter` class. 
  The method is a prototype version. By using this function, plots generated 
  using [object-oriented seaborn interface](https://seaborn.pydata.org/tutorial/objects_interface.html) can be handled as a `patchworklib.Brick(s)` 
  class object.

  #### Parameters

  - `g`: `seaborn.axisgrid.xxGrid` object  
      A return value of the seaborn figure-level plotting functions such as relplot, 
      distplot, catplot, jointplot, and pairplot.
  - `label`: str  
      Unique identifier for the `patchworklib.Brick(s)` class object to be returned. 
      If you want to adjust a layout composed of multiple bricks object using the label 
      indexing method, providing the unique label name for returned object should be 
      encouraged.
  - `figsize`: tuple (float, float)  
      Figure size. If it is not specified, the converted Bricks object will keep the original 
      figure size of the given seaborn plot. 

  #### Returns

  `patchworklib.Bricks` object

- ### **`load_seaborngrid(g, label=None, labels=None, figsize=None)`**

  Load seaborn plot generated based on seaborn.axisgrid.xxGrid class.   
  In general, seaborn plots generated by figure-level function cannot be 
  handled as subplot(s) with other matplotlib plots. However, by processing 
  these seaborn plots via this function, you can handle them as 
  `patchworklib.Brick(s)` class objects. 

  #### Notes

  You should execute `overwrite_axisgrid` function before using this function.

  #### Parameters

  - `g`: seaborn.axisgrid.xxGrid object  
      A return value of the seaborn figure-level plotting functions such as relplot, 
      distplot, catplot, jointplot, and pairplot.
  - `label`: str  
      Unique identifier for the `patchworklib.Brick(s)` class object to be returned. 
      If you want to adjust a layout composed of multiple bricks object using the label 
      indexing method, providing the unique label name for returned object should be 
      encouraged.
  - `figsize`: tuple (float, float)  
      Figure size. If it is  not specified, the converted Bricks object will keep the original 
      figure size of the given seaborn plot. 

  #### Returns

  `patchworklib.Bricks` object

- ### **`hstack(brick1, brick2, target=None, margin=None, direction='r', adjust_height=True, adjust_width=True, va='top')`**

  Align two `patchworklib.Brick(s)` objects horizontally.
  When joining two Brick(s) objects by `"|"` or `"+"` operator, this function is called internally. 

  - `brick1 | brick2` is identical to `hstack(brick1, brick2)`

  - `brick1 + brick2` is identical to `hstack(brick1, brick2, adjust_height=False)`

  #### Parameters

  - `brick1`: patchworklib.Brick or patchworklib.Bricks class object  
      A `patchworklib.Brick(s)` class object to be joined with `brick2` object. 
      The location of this object is used as the base position for determining 
      the `brick2` placement. 
  - `brick2`: patchworklib.Brick or patchworklib.Bricks class object  
      A `patchworklib.Brick(s)` class object to be placed on the side specified by `direction` 
      (by default, on the right side) of the `brick1` object.
  - `target`: str, default: `None`  
      Unique label name of the Brick or Brick(s) object that is a part of the `brick1` 
      object. If you want to place `brick2` object next to the specific Brick(s) object 
      in `brick1` object, please provide the `label` value of the Brick(s) object.
  - `margin`: float or str (`"none"`), default: `None`  
      Margin size between the two given Brick(s) objects. 
      If `None`, the `pw.param["margin"]` value will be used as the margin size. 
      If the value is `"none"`, two Brick(s) objects will be joined with no margin 
      (meaning that the axes spines will be joined so that they are fully glued together). 
  - `direction`: str (`"r"` or `"l"`), default: `"r"`  
      Side on which `brick2` is placed with respect to `brick1`. 
      "r" means right. "l" means left.
  - `adjust_height`: bool, default: `True`  
      If `True`, the height of `brick2` object is adjusted so that it will be equal to the 
      height of `brick1` object.
  - `adjust_width`: bool, default: `True`  
      If `True`, the width of `brick2` will be adjusted according to the aspect of `brick2`
      after stacking. If False, `brick2` will keep its original width after stacking. 
      If `adjust_height` is `False`, the value will also be `False`. 
  - `va`: str (`"top"` or `"bottom"`), default: `False`  
     If `adjust_height` is `False`, the value will be effective. 
     If the value is `"top"`, `brick2` object will be aligned to the left/right top of `brick1` object. 
     If the value is `"bottom"`, `brick2` object will be aligned to the left/right bottom of `brick1` object.

  #### Returns
  
  `patchworlib.Bricks` object



- ### **`vstack(brick1, brick2, target=None, margin=None, direction='t', adjust_height=True, adjust_width=True, ha='l')`**

  Align two patchworlib.Brick(s) objects vertically.
  When joining two Brick(s) objects by "/" or "-" operator, this function is called internally. 

  - `brick2 / brick1` is identical to `vstack(brick1, brick2)`

  - `brick2 / brick1` is identical to `vstack(brick1, brick2, adjust_width=False)`

  #### Parameters

  - `brick1`: patchworklib.Brick or patchworklib.Bricks class object  
      A `patchworlib.Brick(s)` class object to be joined with `brick2` object. 
      The location of this object is used as the base position for determining the `brick2` 
      placement.
  - `brick2`: patchworklib.Brick or patchworklib.Bricks class object  
      A `patchworlib.Brick(s)` class object to be placed on the side specified by `direction` 
      (by default, on the top side) of the `brick1` object.
  - `target`: str, default: `None`  
      Unique label name of the Brick or Brick(s) object that is a part of the `brick1` 
      object. If you want to place `brick2` object next to the specific Brick(s) object 
      in `brick1` object, please provide the `label` value of the Brick(s) object.
  - `margin`: flaot, default: `pw.param["margin"]`  
      Margin size between the two given Brick(s) objects.
      If `None`, the `pw.param["margin"]` value would be used as the margin size. 
      If the value is `"none"`, two `patchworlib.Brick(s)` objects will be joined with no margin 
      (meaning that the axes spines will be joined so that they are fully glued together). 
  - `direction`: str ("t" or "b"), default: `"t"`  
      Side on which `brick2` is placed with respect to `brick1`. 
      `"t"` means top, `"b"` means bottom.
  - `adjust_height`: bool, default: `True`   
      If True, the height of `brick2` will be adjusted according to the aspect of `brick2`
      after stacking. If False, `brick2` will keep its original height after stacking. 
      If `ajust_width` is `Fasle`, the value will also be `False`. 
  - `adjust_width`: bool, default: `True`  
      If `True`, the width of `brick2` object is adjusted so that it will be equal to the width 
      of `brick1` object.
  - `ha`: str ("left" or "right"), default: `False`  
     If `adjsut_width` is `False`, the value will be effective. 
     If the value is `"left"`, `brick2` object will be stacked on/below the left top/bottom of `brick1` object. 
     If the value is `"right"`, `brick2` object will be stacked on/below the right top/bottom of `brick1` object. 

  #### Returns

  `patchworlib.Bricks` object

- ### **`stack(bricks, margin=None, operator='|', equal_spacing=False)`**

  Stack multiple Brick(s) objects horizontally or vetically.

  #### Parameters

  - `bricks`: list of patchworklib.Brick(s) objects   
      List composed of `patchworklib.Brick(s)` objects. 
      The list can include both `patchworklib.Brick` and `patchworklib.Bricks` objects. 
  - `margin`: float, default: `None`  
      Margin size of each Brick(s). If None, the `pw.param["margin"]` value would be 
      used as the margin size between the Brick(s) objects.
  - `operator`: str (`|`, `/`, `+`, or `-`), default: `|`  
      Orientation of the arrangement for the given `patchworklib.Brick(s)` object.
      If this value is `|` or `/`, the width/height of the object to be stacked will be 
      adjusted so that it will be aligned with previous one.
	- `equal_spacing`: bool, default: `False`
      If this value is `True`, axes bounding-boxes should be placed with equal spacing
      between them. Otherwise, depending on the text values of x/y tick labels and x/y
      labels, they may not always be equally spaced. 
  
  #### Returns

  `patchworlib.Bricks` object

- ### **`inset(brick1, brick2, loc='upper right', wratio=0.4, hratio=0.4, vmargin=0.1, hmargin=0.1, alpha=0.0)`**

  Position the brick2 object inside the brick1 object.

  #### Parameters

    - `brick1`: patchworklib.Brick or patchworklib.Bricks class object   
        Brick(s) class object to be joined with `brick2` object. The location of this 
        object is used as the base position for determining the `brick2` placement. 
    - `brick2`: patchworklib.Brick or patchworklib.Bricks class object  
        Brick(s) class object to be placed in specified in `brick1` object. 
    - `position`: str, (`"upper right"`, `"lower rigtht"`, `"upper left"`, `"lower left"`) 
        Position of `brick2` object in `brick1` object. 
    - `wratio`: float, default: 0.1  
        Ratio of the `brick2` width to `brick1` object. 
    - `hratio`: float, default: 0.1  
        Ratio of the `brick2` height to `brick1` object. 
    - `vmargin`: float, default: 0.1  
        Margin from the bottom/top.
    - `hmargin`: float, default: 0.1  
        Margin from the right/left.
    - `alpha`: flaot, default: 0.0  
        Alpha of background of `brick2` object 

  #### Returns

  `patchworlib.Bricks` object 

- ### **`expand(bricks, w, h)`**

  Expand the size of the bricks object.

  #### Parameters

  - `bricks`: patchworklib.Bricks object  
      A `patchworlib.Brick(s)` object to be expand.
  - `w`: int or float  
      Expansion ratio of the width of an axes object.
  - `h`: int or float  
      Expansion ratio of the width of an axes object.

  #### Returns

  `patchworlib.Bricks` object
  
  

## Brick class

Subclass of `matplotlib.axes.Axes` object.
A `patchworklib.Brick` object can be joined with another `patchworklib.Brick(s)` object
using `/`, `|`, `+`, and `-` operators. 

### Parameters

  - `bricks_dict`: dict  
      Dictionaly of `patchworklib.Brick(s)` objects. The label name of the Brick
      object is served as the dictionaly keys.
  - `label`: str  
      Unique identifier for the Bricks class object. The value can be used in layout
      adjustment using label indexing. The value would be assigned to `self.label`.

### Attributes

  - `case`: matplotlib.Axes.axes  
      An invisible axes object surrounding Bricks object excluding common label, legend.
  - `outline`: patchworklib.Bricks  
      A bricks object based on the invisible axes object surrounding all objects in
      the original Bricks object including `case` axes.
  - `label`: str  
      Unique identifier of the Brick class object. If the Brick object is
      incorporated in the other super Bricks objects, the Brick object can be accessed 
      from the super Bricks object by specifying the label name for the super object as 
      `Bricks_object[{label}]`.
  - `bricks_dict`: dict  
      Dictionary with labels of the Brick object as a dictionary key and the corresponding Brick 
      object as a dictionary value.

### Methods


- ### **`set_index(self, index, x=None, y=None, **args)`**

  This function is the wrapper function of `self.case.text`.
  Set a index label on 'upper left' of the Bricks object. An index labels can be 
  added, such as those on sub-figures published in scientific journals.  

  #### Parameters  
    - `index`: str  
        index value
    - `x`: float  
        By default, the value will be adjusted as index label is placed on 'upper left'
        of the Bricks object.
    - `y`: flaot  
        By default, the value will be adjusted as index label is placed on 'upper left'
        of the Bricks object.
    - `args`: dict  
        Text properties control the appearance of the label.

  #### Returns

  `matplotlib.text.Text` object

- ### **`set_text(self, x, y, text, **args)`**

  

- ### **`add_colorbar(self, cmap=None, x=None, y=None, vmin=0, vmax=1, hratio=None, wratio=None, coordinate='relative', **args)`**

  Set a  colorbar for the Brick object and return a new Bricks object including the colorbar.

  ### Parameters
    - `cmap`: Colormap, default: `'viridis'`  
        The colormap to use.
    - `x`: float, default: `None`  
        if `args['orientation']` is `'vertical'`, the value will be adjusted as the colorbar
        is placed on `'lower right'` of the Bricks object. if `args['orientation']` is
        `'horizontal'`, the value will be adjusted as the colorbar is placed on `'lower center'`
        of the Bricks object. The zero position for `x` is the most left axes of the Brick objects 
        in the Bricks object.
    - `y`: float, default: `None`  
        if `args['orientation']` is `'vertical'`, the value will be adjusted as the colorbar
        is placed on `'lower right'` of the Bricks object. if `args['orientation']` is
        `'horizontal'`, the value will be adjusted as the colorbar is placed on 'lower center'
        of the Bricks object. The zero position for `y` is the most bottom axes of the Brick objects 
        in the Bricks object.
    - `vmin`: float, default: `0`  
        Minimum value to anchor the colormap.
    - `vmax`: float, default: `1`  
        Maximum value to anchor the colormap.
    - `hratio`: float  
        Height ratio of colorbar to height of `self.case`.
    - `wratio`: float  
        Width ratio of colorbar to width of `self.case`.
    - `coordinate`: str (`"relative"` or `"absolute"`), default `"relative"`  
        If `"absolute"`, the values of x and y will mean the inches of the distances from the
        base zero positions. if `"relative"`, the values of `x` and `y` will mean the relative
        distances based on width and height of Bricks object from the base zero positions.

  #### Returns

  `patchworklib.Bricks` object

- ### **`move_legend(self, new_loc, **kws)`**

  Move legend

  ### Parameters
    - `new_lock`: str or int  
        Location argument, as in matplotlib.axes.Axes.legend().
    - `kws`: dict  
        Other keyword arguments can be used in matplotlib.axes.Axes.legend().

  #### Returns

  `None`

- ### **`get_inner_corner(self, labels=None)`**

  Return the most left, right, bottom, and top positions of the Brick objects.

  #### Returns

  `tuple` (left, right, bottom, top)

- ### **`get_middle_corner(self, labels=None)`**

  Return the left, right, bottom, and top positions of `self.case`.
  `patchworklib.Brick` object provides `case` attribute. `case` is invisible `matplotlib.axes.Axes`
  object surrounding the Brick object and their artist, text objects.

  #### Returns

  `tuple` (left, right, bottom, top)

- ### **`get_outer_corner(self)`**

  Return the left, right, bottom, and top positions of `self.outline`.
  A `patchworklib.Brick` object provides `outline` attribute. `outline` is Bricks object based on
  invisible `matplotlib.axes.Axes` object surrounding `case` axes, and its artist, text objects.

  #### Returns

  `tuple` (left, right, bottom, top)

## Bricks class

A `patchworklib.Bricks` class object is a collection of `patchworklib.Brick` object.
It can be joined with aother `patchworklib.Brick(s)` object by using `/`, `|`, `-`, and `+` operators.


  ### **Parameters**
  - `bricks_dict`: dict  
      Dictionaly of patchworklib.Brick class objects. The label name of each Brick 
      object is served as the dictionaly keys. 
  - `label`: str  
      Unique identifier for the Bricks class object. The value can be used in layout 
      adjustment using label indexing. The value would be assigned to `self.label`.

  ### **Attributes**  
  - `case`: matplotlib.Axes.axes  
      Invisible axes object surrounding Bricks object excluding common label, legend.
  - `outline`: patchworklib.Bricks  
      A `patchworklib.Bricks` object based on the invisible axes object surrounding all 
      objects in the original Bricks object including `case` axes.
  - `label`: str  
      Unique identifier of the Bricks class object. If the Bricks object is 
      incorporated in the other super Bricks objects, by specifying the label name for 
      the super object as `Bricks_object[{label}]`, the Bricks object can be accessed 
      from the super Bricks object.
  - `bricks_dict`: dict  
      Dictionary with labels of the `patchworklib.Brick(s)` objects in the Bricks object as 
      dictionary keys and the corresponding `patchworklib.Brick(s)` objects as dictionary values.

  ### **Methods**
- ### **`set_supxlabel(self, xlabel, labelpad=None, *, loc=None, **args)`**

  This function is the wrapper of `self.case.set_xlabel`. 
  Set a common xlabel for the Bricks objects. If a Bricks class object is composed of 
  multiple Brick(s) class objects and each has same xaxis labels,  you can remove the
  redundant labels and can add the common x axis label for all Brick(s) objects. 
  #### Parameters
  
  - `xlabel`: str  
      xlabel value 
  - `labelpad`: int, default: `8`  
      Spacing in points from the virtual axes bounding box of the Bricks object.
  - `args`: dict  
      Text properties control the appearance of the label.
  
  #### Returns
  
  `matplotlib.text.Text` object
  
- ### **`set_supylabel(self, ylabel, labelpad=None, *, loc=None, **args)`**

  This function is the wrapper of `self.case.set_ylabel`.
  Set a common ylabel for the Bricks objects. If a Bricks class object is composed of 
  multiple Brick(s) class objects and each has same yaxis labels, you can remove the
  redundant labels and can add the common y axis label for all Brick(s) objects. 

  #### Parameters

  - `ylabel`: str  
      ylabel value 
  - `labelpad`: int, default: `8`  
      Spacing in points from the virtual axes bounding box of the Bricks object.
  - `args`: dict  
      Text properties control the appearance of the label.

  #### Returns

  `matplotlib.text.Text` object

- ### **`set_suptitle(self, title, loc=None, pad=None, **args)`**

  This function is the wrapper of `self.case.set_title`.
  Set a common title for the Brick(s) objects. If a Bricks class object is composed of 
  multiple Brick(s) class objects and each has common title, you can set common title 
  for all Brick(s) objects. 

  #### Parameters

  - `title`: str  
      title value 
  - `loc`: str (`"center"`, `"left"`, `"right"`), default `"center"`
      Which title to set.  
  - `pad`: int, default: `12`  
      Spacing in points from the virtual axes bounding box of the Bricks object.
  - `args`: dict  
      Text properties control the appearance of the label.

  #### Returns

  `matplotlib.text.Text` object 

- ### **`set_index(self, index, x=None, y=None, **args)`**

  This function is the wrapper function of `self.case.text`.
  Set a index label on 'upper left' of the Bricks object. An index labels can 
  be added, such as those on sub-figures published in scientific journals. 

  #### Parameters
    - `index`: str  
        index value 
    - `x`: float  
        By default, the value will be adjusted as index label is placed on 'upper left' 
        of the Bricks object. 
    - `y`: flaot  
        By default, the value will be adjusted as index label is placed on 'upper left' 
        of the Bricks object.
    - `args`: dict  
        Text properties control the appearance of the label.

  **Returns**

  `matplotlib.text.Text` object

- ### **`set_text(self, x, y, text, **args)`**

- ### **`set_supspine(self, which='left', visible=True, position=None, bounds=None)`**

  Set a common spine for the Bric(s) objects in the Bricks object.   
  The spines of `self.case` surrounding the Bricks object are invisible by default. 
  However, by applying this methods, a specified spine will be visible.

  #### Parameters

  - `which`: str ('left', 'right', 'top', 'bottom'), default: `"left"`  
      Kind of the spine 
  - `visible`: bool, default: `True`  
      Setting of Show/hide the spine
  - `position`: tuple (position type ('outward', 'axes', 'data'), amount (float))  
      Position of the spine.
      For details, please see 'https://matplotlib.org/3.5.1/api/spines_api.html'.
  - `bounds`: tuple (float, float)  
      Bounds of the spine. 
      For details, please see 'https://matplotlib.org/3.5.1/api/spines_api.html'.

  #### Returns

  `matplotlib.spines.Spine` object

  
- ### **`align_xlabels(self, keys=None)`**

	Align the xlabels of the specified Brick objects.
	
	#### Parameters
	
	- `keys`: `list` of `str` or `Brick` object
   The xlabels of the Brick objects specified by `keys` will be aligned. 

- ### **`align_ylabels(self, keys=None)`**

	Align the ylabels of the specified Brick objects.
	
	#### Parameters
	
	- `keys`: `list` of `str` or `Brick` object
   The ylabels of the Brick objects specified by `keys` will be aligned.

- ### **`add_colorbar(self, cmap=None, x=None, y=None, vmin=0, vmax=1, hratio=None, wratio=None, coordinate='relative', **args)`**

  Set a common colorbar for the Bricks objects and return a new Bricks object including the colorbar.
  
  #### 	Parameters
  
  - `cmap`: Colormap, default: `'viridis'` 
   The colormap to use.
  - `x`: float, default: `None`  
   if args['orientation'] is 'vertical', the value will be adjusted as the colorbar 
    is placed on 'lower right' of the Bricks object. if args['orientation'] is 
    'horizontal', the value will be adjusted as the colorbar is placed on 'lower center' 
    of the Bricks object. The zero position for `x` is the most left axes of the Brick 
    objects in the Bricks object.
  - `y`: float, default: `None`  
   if args['orientation'] is 'vertical', the value will be adjusted as the colorbar 
    is placed on 'lower right' of the Bricks object. if args['orientation'] is 
    'horizontal', the value will be adjusted as the colorbar is placed on 'lower center' 
    of the Bricks object. The zero position for `y` is the most bottom axes of the 
    Brick objects in the Bricks object.
  - `vmin`: float, default: `0`  
   Minimum value to anchor the colormap.
  - `vmax`: float, default: `1`  
   Maximum value to anchor the colormap.
  - `hratio`: float   
   Height ratio of colorbar to height of `self.case`
  - `wratio`: float   
   Width ratio of colorbar to width of `self.case`
  - `coordinate`: str ("relative", "absolute"), default: `"relative"`  
   if "absolute", the values of x and y will mean the inches of the distances from the 
    base zero positions. if "relative", the values of x and y will mean the relative 
    distances based on width and height of Bricks object from the base zero positions.
  
  #### Returns
  
  `patchworklib.Bricks` object
  
- ### **`move_legend(self, new_loc, **kws)`**

  Move legend.

  #### Parameters
    - `new_lock`: str or int  
        Location argument, as in matplotlib.axes.Axes.legend().
    - `kws`: dict  
        Other keyword arguments can be used in matplotlib.axes.Axes.legend().

  #### Returns
  `None`


- ### **`get_inner_corner(self)`**

    Return the the left, right, bottom, and top positions .
    The inner corners of the Bricks object means the most top left, top right, bottom left, 
    and bottom right corners of the spines of Brick objects in the Bricks object.

    #### Returns

    `tuple` (left, right, bottom, top)

- ### **`get_middle_corner(self)`**

    Return the left, right, bottom, and top positions of `self.case`.
    A `patchworklib.Bricks` object provides `case` attribute. A `case` is invisible
     `matplotlib.axes.Axes` object surrounding Brick objects in the Bricks object 
    and their artist, text objects.

    #### Returns

    `tuple` (left, right, bottom, top)

- ### **`get_outer_corner(self)`**

    Return the left, right, bottom, and top positions of `self.outline`.
    A `patchworklib.Bricks` object has `outline` attribute. It is a Bricks object based on
    invisible matplotlib.axes.Axes object surrounding `case` axes, and its artist, text objects.   

    #### Returns

    `tuple` (left, right, bottom, top)

  
