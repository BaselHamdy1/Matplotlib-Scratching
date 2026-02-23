2026-02-21 19:14
Tags: [[Visuals]]

# Matplotlib

### What is Matplotlib?

Matplotlib is a used Python library used for creating static, animated and interactive data visualizations. It is built on the top of NumPy and it can easily handles large datasets for creating various types of plots such as line charts, bar charts, scatter plots, etc.

### 1.Basics and Line Plots

- **Importing**
```
from matplotlib import pyplot as plt
```

- **Generating the plot**
```
plt.plot(x_values, y_values)
```

- **Customizing Labels and Titles**
```
- plt.ylabel()
  plt.xlabel()
  plt.title()
```

- **Legend**
  You can pass a list of labels to `plt.legend()`, but this requires knowing the exact order in which lines were plotted.

    ◦ A better method is to add a `label` argument directly to each `plt.plot()` call (e.g., `label='Python'`) and then call `plt.legend()` without arguments. This makes the code self-documenting and less error-prone

- **Line and Colors**

  Format Strings: method like `'k--'` (black dashed line) can be used, but it is often difficult to read.

    ◦ Explicit Arguments: It is clearer to use keywords like `color='k'`, `linestyle='--'`, and `marker='.'`.

    ◦ Hex Codes: For more precise color choices, hex values (e.g., `color='#5a7d9a'`) are supported.

  • Line Width: You can emphasize specific lines by increasing the `linewidth` (e.g., `linewidth=3`).

- **Layout**
  Grid: Adding `plt.grid(True)` makes it easier to track when lines cross specific values

  Padding: `plt.tight_layout()` is a helpful method to automatically adjust plot parameters and padding so that labels are not cut off

• **Built-in Themes:**

 You can view available styles and apply them with
```
 plt.style.available
 plt.style.use('style_name')
```

- **Special Styles**

```
plt.xkcd()
```

- **Saving Plots** 
```
  plt.savefig('filename.png')
```

**Filling between lines**
```
plt.plot(days, y1, label='Line1')  
plt.plot(days, y2, label='Line2')  
  
plt.fill_between(days, y1, y2, color='lightgreen', alpha=0.5)  
  
plt.legend()
```

### 2.Bar Charts

- **Importing**:
  You can pass arguments like `color` and `label`
   When multiple `plt.bar()` calls are made, Matplotlib stacks them on top of each other, which often makes data difficult to read
```
plt.bar(x_values, y_values)
```

- **Plotting side by side**
  To display multiple categories (e.g., different developer types) side-by-side, you must manually offset the X-axis positions using **NumPy**

Workflow:
1. **Create an index**: Use `np.arange(len(x_values))` to create a numerical array representing your categories.

2. **Define Width**: Set a `width` variable (`0.25`) to control bar width.

3. **Shift Positions**:

    ◦ Bar 1: `x_indexes - width`

    ◦ Bar 2: `x_indexes`

    ◦ Bar 3: `x_indexes + width`.

4. **Fix X-Axis Labels**: Because you are now plotting against an index, you must restore the original labels using `plt.xticks(ticks=x_indexes, labels=original_list)`.

### 3.Horizontal Bar Charts
Horizontal charts are preferred when dealing with a large number of categories or long label names that would look crowded on a vertical axis

- **Syntax** 
```
plt.barh(y_values, x_values)
```

• **Axis Swap**: When switching from `bar` to `barh`, ensure you update your `xlabel` and `ylabel` as the data orientation has flipped.

• **Ordering**: If you want the most popular items at the top, use the `.reverse()` method on your data lists before plotting.

### 4. Analyzing Data from CSVs

When data is stored externally, you must load and process it before plotting. The source highlights two primary methods:

 Method A: Standard Library (`csv` module)

This approach uses the built-in `csv.DictReader` to iterate through rows as dictionaries.

• **Cleaning Data**: If a column contains multiple values (languages separated by semicolons), use `.split(';')` to turn them into a list.

• **Counting Frequency**: Use `collections.Counter` to track the popularity of items.

• **Top Results**: The `.most_common(n)` method efficiently retrieves the top `n` items for plotting.

Method B: Pandas 

**Pandas** is faster and requires less code for reading CSVs.

• **Loading**: Use `pd.read_csv('filename.csv')`.

• **Accessing Columns**: You can grab specific columns directly (`data['column_name']`) and iterate through them to update your `Counter`

### 5.Pie Chart

- **1-Basic Syntax**
```
  plt.pie(slices)
```
Values in the slices list do not need to add up to 100 or any specific number. Matplotlib automatically calculates the proportions based on the sum of all values provided

- **2-Labels and Styling**

  • **Labels**: Pass a list of strings to the `labels` argument.

  • **Wedge Borders**: To add a separator between slices (instead of solid colors bumping into each other), use the `wedgeprops` argument with a dictionary
```
  plt.pie(slices, wedgeprops={'edgecolor': 'black'})
```

• **Custom Colors**: You can define a list of colors (names or Hex values) and pass them to the `colors` argument. Using **Hex values** is recommended for a more professional, muted look compared to bright built-in colors

**3-Emphasis and Rotation**
You can highlight specific data points or change the orientation of the chart for better visual flow.

• **Exploding Slices**: To "pop out" or emphasize a specific slice, use the `explode` argument.

   Pass a list of floats where `0` represents no offset and a value like `0.1` represents 10% of the radius exploded out.

• **Shadows**: Add `shadow=True` to give the chart a slightly 3D effect.

• **Starting Angle**: Use `startangle` to rotate the chart. For example, setting it to `90` will start the first slice at the top (the 12 o'clock position)

**4-Displaying Percentage**

To show the actual percentage of each slice directly on the chart, use the `autopct` argument

```
autopct='%1.1f%%'
```

This format string tells Matplotlib to display the percentage value as a float with one decimal place

|Function|Purpose|
|---|---|
|`plt.pie()`|Generates a pie chart.|
|`wedgeprops={}`|Customizes the appearance of slice edges.|
|`explode=[]`|Offsets specific slices for emphasis.|
|`autopct=''`|Formats and displays percentages on slices.|
|`startangle=`|Rotates the entire chart by a specified degree.|

### 6.Stack Plots

**1. Fundamental Concept**

Stack plots (also called **Area Plots**) are used to show how multiple data sets contribute to a whole **over time**.  
Instead of drawing separate line plots, the values are **stacked on top of each other**, forming filled areas.

- **Core Idea:**  
    Each dataset starts where the previous one ends, not from zero.

- **Why Stack Plot:**  
    It helps visualize:
    
    - Overall total trend
        
    - Individual contribution of each category
        
- **Basic Syntax:**  
```
  plt.stackplot(x, y1, y2, y3)
```

**2. Data Structure**

Stack plots require a specific data organization.

- **X-axis:**  
    Usually represents time or sequence (days, years, hours, etc.)
    
- **Y-values:**  
    Multiple lists or arrays, each representing one category.
    
- **Important Rule:**  
    All Y-lists must have the **same length** as the X list.


 **3. Labels and Legends**

Since multiple datasets are stacked together, labels are essential.

- **Labels:**  
    Pass a list of names using the `labels` argument.
    
- **Legend:**  
    Use `plt.legend()` to clearly identify each stacked  area.
    
- **Best Practice:**  
    Always include a legend to avoid confusion.
    

---

**4. Colors and Visual Clarity**

Stack plots rely heavily on colors to distinguish areas.

- **Custom Colors:**  
    You can pass a list of colors using the `colors` argument.
    
- **Why Custom Colors:**
    
    - Improves readability
        
    - Prevents similar colors from blending together
        
- **Recommendation:**  
    Use soft or muted colors for professional-looking plot


### 7.Histogram

**1. Overview**

Histograms are used to visualize the **distribution** of data by showing where values fall within certain boundaries. While they resemble bar charts, histograms group data into **bins** instead of plotting each individual value, which is particularly useful for datasets with many unique numerical values (like ages).

• **Why use Histograms over Bar Charts?** In a survey with ages ranging from 1 to 100, a bar chart would create 100 individual columns, which is difficult to read. A histogram simplifies this by grouping those ages into ranges.

**2. Defining Bins**

Bins determine how data is grouped. You can define them in two ways:

• **Integer Approach**: Passing an integer (e.g., `bins=5`) automatically divides the entire range of data into that number of equal-width bins.

• **List Approach**: Passing a list (e.g., `bins=`) allows for manual control over the exact boundaries.

    ◦ This method is often preferred because it makes the chart easier to read and allows you to exclude specific data by omitting certain ranges.

**3. Visual Customization**

• **Edge Color**: By default, histogram bins can "run together". Adding `edgecolor='black'` makes the individual bins distinct and easier to count.

• **Logarithmic Scale**: When certain values are significantly larger than others—causing smaller data points to disappear—use `log=True`. This plots the data on a logarithmic scale, making small distributions visible even when dwarfed by large numbers.

4. Overlaying Statistical Markers

You can add additional information to a histogram, such as a vertical line representing the **median** value of the dataset.

• **Vertical Lines**: Use `plt.axvline(median_value)` to draw a vertical line across the axes.

• **Customization**:

```
    ◦ Color: color='#fc4f30' (or any hex/name).

    ◦ Label: label='Age Median' (use with plt.legend() to identify the line).

    ◦ Thicknes: linewidth=2 to ensure the line doesn't obstruct the underlying data.
```

**5.Key Functions**

| Function                     | Purpose                                                                    |
| ---------------------------- | -------------------------------------------------------------------------- |
| `plt.hist(data, bins=n)`     | Creates a histogram with _n_ automatically calculated bins.                |
| `plt.hist(data, bins=[...])` | Creates a histogram with specific, manually defined bin ranges.            |
| `log=True`                   | (Argument) Switches to a logarithmic scale for extreme data distributions. |
| `plt.axvline(x)`             | Adds a vertical line at a specific point on the x-axis.                    |
| `plt.legend()`               | Displays the labels for the histogram and any overlaid lines               |

### 8.Scatter Plots

**1. Fundamental Concept**

Scatter plots are used to show the **relationship** between two sets of values to see if they are **correlated**. They are particularly effective for identifying trends, clusters, or **outliers** within a dataset.

• **Correlation**: If the dots form a visible pattern (like a line), the values are likely correlated; if they appear random, there is likely no correlation.

• **Outliers**: Data points that fall far away from the main cluster (a viral video with significantly more views than others) are easily spotted in a scatter plot

**2. Basic Plotting and Customization**

- Basic Syntax
```
- plt.scatter(x, y, s=100, c='green'/'Hex code', marker='x', edge color='black', line width=1, alpha=0.75)
  
```
alpha : soften colors and see overlapping colors

**3. Adding More Dimensions (Colors & Sizes)**

- **Per-Mark Color**: Instead of a single color, pass a **list of values** to the `c` argument. Matplotlib will assign colors based on those values like representing survey satisfaction levels
-  **Color Maps** (cmap) : Use built-in color maps like `'Greens'` or `'summer'` to change the intensity or palette of the data points.

- **Color Bar**: Always add a color bar legend using `plt.colorbar()` so viewers understand what the different shades represent a satisfaction level or a like/dislike ratio

-  **Per-Mark Size**: You can also pass a list of values to the `s` argument to scale the dots based on another metric, such as population or sample size.

**4. Handling Outliers with Log Scales**

When a dataset contains extreme outliers, the standard scale can bunch most of the data into a tiny corner.

• **Logarithmic Scaling**: Use `plt.xscale('log')` and `plt.yscale('log')` to use a log scale on the axes.

• **Benefit**: This prevents outliers from skewing the entire plot, making the correlation between the rest of the data points much easier to see

| Function            | Purpose                                                              |
| ------------------- | -------------------------------------------------------------------- |
| `plt.scatter(x, y)` | Creates a basic scatter plot.                                        |
| `s=`                | Argument to set marker size (can be a value or a list).              |
| `c=`                | Argument to set marker color (can be a color or a list for mapping). |
| `cmap=''`           | Argument to define the color palette for mapped values.              |
| `plt.colorbar()`    | Adds a color scale legend to the side of the plot.                   |
| plt.xscale('log')   | Switches the x-axis to a logarithmic scale to handle outliers        |

### 9.Time Series

**1. Fundamental Concept**

Time series data involves plotting values against specific dates or times. Because dates are often crowded or out of order when loaded from external files, Matplotlib provides specialized tools to format, rotate, and align them for better readability.

**2. Plotting Dates**

The primary method for plotting time-based data is `plt.plot_date()`.

• **Default Behavior**: By default, `plot_date` displays data points as markers without a connecting line.

• **Connecting with Lines**: To add a line, set the `linestyle` argument `linestyle='solid'`

• **Markers**: You can remove markers by setting `marker=None` or customize them as you would with standard plots

**3. Formatting and Layout**

Dates often "bunch together" on the x-axis, making them impossible to read. You can fix this using the following techniques:

-  **Auto-Formatting**: Use `plt.gcf().autofmt_xdate()` (Get Current Figure). This automatically rotates the dates and adjusts their alignment so they fit nicely without overlapping.

-  **Custom Date Formats**: To change how dates appear (e.g., "May 24, 2019" instead of "2019-05-24"), use the `DateFormatter` class from `matplotlib.dates`.

- **Formatting Codes**: Use standard Python datetime strings like `%b` (abbreviated month), `%d` (day), and `%Y` (4-digit year)

- **Applying the Format**: You must grab the current axis using `plt.gca()` and then use `set_major_formatter` on the x-axis

**4. Handling Dates in Pandas and CSVs**

When loading dates from a CSV file, they are often read as **strings** rather than date objects, which can cause them to plot out of chronological order.

• **Converting to Datetime**: Use the Pandas method `pd.to_datetime()` to convert a column of strings into actual datetime objects.

• **Sorting**: Once converted, use `df.sort_values()` (with `inplace=True`) to ensure the dates are in the correct chronological order before plotting. This prevents the plot from jumping back and forth across the timeline

|Function|Purpose|
|---|---|
|`plt.plot_date(x, y)`|Plots data where the x-axis consists of date objects.|
|`plt.gcf().autofmt_xdate()`|Automatically rotates and aligns x-axis date labels.|
|`mpl_dates.DateFormatter()`|Defines a custom display format for dates (e.g., Month Day, Year).|
|`plt.gca().xaxis.set_major_formatter()`|Applies a defined date format to the current plot's x-axis.|
|`pd.to_datetime(df['col'])`|(Pandas) Converts string data into datetime objects for proper plotting.|
|`df.sort_values('col')`|(Pandas) Ensures date data is plotted in chronological order.|
### 10.Plotting live data

**1.Overview**

Real-time plots are essential for monitoring data that changes frequently, such as **YouTube subscriber counts**, **sensor readings**, or **API feedback**. Instead of a static image, the plot updates dynamically as new data becomes available.

**2. Core Mechanism:** `FuncAnimation`

To create an animated plot, you must use the `FuncAnimation` class from the `matplotlib.animation` module.

• **Import**: `from matplotlib.animation import FunkAnimation`.

• **The** **animate** **Function**: You define a custom function (often called `animate`) that contains the logic for what should happen during each update.

• **The Animation Object**: You create an instance of `FuncAnimation` by passing specific arguments:

 ◦ **Figure**: The figure to animate, retrieved via `plt.gcf()` (Get Current Figure).

   **Function**: The name of your custom `animate` function.

   **Interval**: How often to run the function, in **milliseconds** (`1000` for once per second)

**3. The "Stacking" Problem and `plt.cla()`**

A common issue in live plotting is that Matplotlib’s `plot()` method adds a brand-new line every time it is called without removing the old ones. This causes different colored lines to stack on top of each other, making the chart look incorrect.

• **Solution**: Use `plt.cla()` (**Clear Axis**) at the beginning of your `animate` function.

• **Effect**: This clears the current axis before re-plotting the data from scratch, ensuring only the most recent data is displayed in a consistent style.

**4. Working with External Data (CSV)**

For real-world applications, data is often generated by an outside source (like a sensor or API) and written to a **CSV file**.

• **Inside the** **animate** **Function**: The script should read the CSV file during every update interval.

• **Data Processing**: Use **Pandas** to read the file and assign columns to your X and Y variables.

• **Persistence**: As long as the external source continues appending data to the CSV, Matplotlib will pick up those changes and update the plot in real-time.

**5. Refining the Live Plot**

Because `plt.cla()` clears the entire axis, certain elements must be redefined _inside_ the `animate` function to persist:

• **Legends**: Re-call `plt.legend()` after plotting.

   Tip: Set a specific location (e.g., `loc='upper left'`) to prevent the legend from "jumping" around as Matplotlib tries to find the best fit for changing data.

• **Padding**: Call `plt.tight_layout()` inside the function to ensure labels are always adjusted correctly.

• **Labels**: Ensure titles and axis labels are re-applied if they were cleared

### 11.Supplots

**1. Stateful vs. Object-Oriented Approach**

So far, most plotting has used the **stateful** approach, where you import the `pyplot` module (as `plt`) and call methods directly on it (e.g., `plt.plot()`). While simple, this approach assumes you are always working with the "current" figure and axis.

The **Object-Oriented (OO)** approach is preferred for complex visualizations because it allows you to explicitly instantiate figure and axis objects. Many developers prefer this even for single plots because it is more explicit.

• **Figure**: The overall container for your plots (the entire window).

• **Axes**: The actual individual plots within that figure.

**2. Creating Subplots**

To use the OO approach, you instantiate your objects using `plt.subplots()`.

• **Single Plot (Default)**: `fig, ax = plt.subplots()` creates one figure and one axis (a 1x1 grid).

• **Multiple Plots (Grid)**: You can specify the number of rows and columns: `fig, ax = plt.subplots(nrows=2, ncols=1)`.

• **Returning Axes**:

   If you have one plot, `ax` is a single axis object.

   If you have multiple plots, `ax` is returned as a **list** (for 1D grids) or a **nested list/matrix** (for 2D grids).

   **Unpacking**: It is common to "unpack" these for easier access: `fig, (ax1, ax2) = plt.subplots(nrows=2, ncols=1)`.

**3. Syntax Differences (OOP vs. Stateful)**

When switching to the object-oriented approach, several common methods use slightly different names:

|Stateful Method (`plt`)|Object-Oriented Method (`ax`)|
|---|---|
|`plt.plot()`|`ax.plot()`|
|`plt.legend()`|`ax.legend()`|
|`plt.title()`|`ax.set_title()`|
|`plt.xlabel()`|`ax.set_xlabel()`|
|`plt.ylabel()`|`ax.set_ylabel()`|

**4. Cleaning Up the UI**

When plotting multiple subplots in the same figure, the axes can become cluttered with redundant labels.

• **sharex=True**: Passing this to `plt.subplots()` will only label the bottom-most x-axis ticks, which is useful when all plots share the same x-axis (e.g., age or time).

• **sharey=True**: Only labels the left-most y-axis ticks for multiple columns.

**5. Multiple Separate Figures**

If you want your plots to appear in **separate windows** rather than subplots within the same window, you create multiple figure objects.

• **Example**:

• This allows you to treat them as completely independent visualizations

| Function                   | Purpose                                               |
| -------------------------- | ----------------------------------------------------- |
| `plt.subplots(rows, cols)` | Instantiates figure and axis objects for OO plotting. |
| `ax.set_title()`           | Sets the title for a specific axis.                   |
| `ax.set_xlabel()`          | Sets the x-axis label for a specific axis.            |
| `sharex=True`              | Removes redundant x-axis tick labels across subplots. |
| `fig.savefig('name.png')`  | Saves a specific figure object to a file              |

Resources: https://youtu.be/UO98lJQ3QGI?si=_KZGQ1G_cYF-E9Og - [Data Visualization using Matplotlib in Python - GeeksforGeeks](https://www.geeksforgeeks.org/data-visualization/data-visualization-using-matplotlib/)

 