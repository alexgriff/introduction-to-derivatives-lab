
# Derivatives of Linear Functions Lab- Update -hjghgh

### Introduction: Start here

In this lab, we will practice our knowledge of derivatives. Remember that our key formula for derivatives, is 
$f'(x) = \frac{\Delta y}{\Delta x} =  \frac{f(x + \Delta x) - f(x)}{\Delta x}$.  So in driving towards this formula, we will do the following: 

1. Learn how to represent linear and nonlinear functions in code.  
2. Then because our calculation of a derivative relies on seeing the output at an initial value and the output at that value plus delta x, we need an `output_at` function.  
3. Then we will be able to code the $\Delta f$ function that sees the change in output between the initial x and that initial x plus the $\Delta x$ 
4. Finally, we will calculate the derivative at a given x value, `derivative_at`. 

### Learning objectives 

For this first section, you should be able to answer all of the question with an understanding of our definition of a derivative:

1. Our intuitive explanation that a derivative is the instantaneous rate of change of a function
2. Our mathematical definition is that 

$f'(x) = \frac{\Delta y}{\Delta x} =  \frac{f(x + \Delta x) - f(x)}{\Delta x}$

### Let's begin: Starting with functions

#### 1. Representing Functions

We are about to learn to take the derivative of a function in code.  But before doing so, we need to learn how to express any kind of function in code.  This way when we finally write our functions for calculating the derivative, we can use them with both linear and nonlinear functions.

For example, we want to write the function $f(x) = 2x^2 + 4x - 10 $ in a way that allows us to easily determine the exponent of each term.

This is our technique: write the formula as a list of tuples.  

> A tuple is a list whose elements cannot be reassigned.  But everything else, for our purposes, is the same.  
```python
tuple = (7, 3)
tuple[0] # 7
tuple[1] # 3
```

> We get a TyperError if we try to reassign the tuple's elements.
```python
tuple[0] = 7
# TypeError: 'tuple' object does not support item assignment
```

Take the following function as an example: 

$$f(x) = 4x^2 + 4x - 10 $$

Here it is as a list of tuples:


```python
four_x_squared_plus_four_x_minus_ten = [(4, 2), (4, 1), (-10, 0)]
```

So each tuple in the list represents a different term in the function.  The first element of the tuple is the term's constant and the second element of the tuple is the term's exponent.  Thus $4x^2$ translates to `(4, 2)` and  $-10$ translates to `(-10, 0)` because $-10$ equals $-10*x^0$.  
> We'll refer to this list of tuples as "list of terms", or `list_of_terms`.

Ok, so give this a shot. Write $ f(x) = 4x^3 + 11x^2 $ as a list of terms.  Assign it to the variable `four_x_cubed_plus_eleven_x_squared`.


```python
four_x_cubed_plus_eleven_x_squared = [(4, 3), (11, 2)]
```

#### 2. Evaluating a function at a specific point 

Now that we can represent a function in code, let's write a Python function called `term_output` that can evaluate what a single term equals at a value of $x$.  

* For example, when $x = 2$, the term $3x^2 = 3*2^2 = 12 $.  
* So we represent $3x^2$ in code as `(3, 2)`, and: 
* `term_output((3, 2), 2)` should return 12



```python
def term_output(term, input_value):
    pass
```


```python
term_output((3, 2), 2) # 12
```




    12



> **Hint:** To raise a number to an exponent in python, like 3^2 use the double star, as in:
```python
3**2 # 9 
```

Now write a function called `output_at`, when passed a `list_of_terms` and a value of $x$, calculates the value of the function at that value.  
> * For example, we'll use `output_at` to calculate $f(x) = 3x^2 - 11$.  
> * Then `output_at([(3, 2), (-11, 0)], 2)` should return $f(2) = 3*2^2 - 11 = 1$


```python
def output_at(list_of_terms, x_value):
    outputs = list(map(lambda term: term_output(term, x_value), list_of_terms))
    return sum(outputs)

```


```python
three_x_squared_minus_eleven = [(3, 2), (-11, 0)]
output_at(three_x_squared_minus_eleven, 2) # 1 
output_at(three_x_squared_minus_eleven, 3) # 16
```




    16



Now we can use our `output_at` function to display our function graphically.  We simply declare a list of `x_values` and then calculate `output_at` for each of the `x_values`.


```python
import plotly
from plotly.offline import iplot, init_notebook_mode
init_notebook_mode(connected=True)

from graph import plot, trace_values

x_values = list(range(-30, 30, 1))
y_values = list(map(lambda x: output_at(three_x_squared_minus_eleven, x),x_values))

three_x_squared_minus_eleven_trace  = trace_values(x_values, y_values, mode = 'lines')
plot([three_x_squared_minus_eleven_trace], {'title': '3x^2 - 11'})
```


<script>requirejs.config({paths: { 'plotly': ['https://cdn.plot.ly/plotly-latest.min']},});if(!window.Plotly) {{require(['plotly'],function(plotly) {window.Plotly=plotly;});}}</script>



<script>requirejs.config({paths: { 'plotly': ['https://cdn.plot.ly/plotly-latest.min']},});if(!window.Plotly) {{require(['plotly'],function(plotly) {window.Plotly=plotly;});}}</script>



<div id="b73364b6-8211-4eea-972c-2d3d9a730aac" style="height: 525px; width: 100%;" class="plotly-graph-div"></div><script type="text/javascript">require(["plotly"], function(Plotly) { window.PLOTLYENV=window.PLOTLYENV || {};window.PLOTLYENV.BASE_URL="https://plot.ly";Plotly.newPlot("b73364b6-8211-4eea-972c-2d3d9a730aac", [{"mode": "lines", "name": "data", "text": [], "x": [-30, -29, -28, -27, -26, -25, -24, -23, -22, -21, -20, -19, -18, -17, -16, -15, -14, -13, -12, -11, -10, -9, -8, -7, -6, -5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29], "y": [2689, 2512, 2341, 2176, 2017, 1864, 1717, 1576, 1441, 1312, 1189, 1072, 961, 856, 757, 664, 577, 496, 421, 352, 289, 232, 181, 136, 97, 64, 37, 16, 1, -8, -11, -8, 1, 16, 37, 64, 97, 136, 181, 232, 289, 352, 421, 496, 577, 664, 757, 856, 961, 1072, 1189, 1312, 1441, 1576, 1717, 1864, 2017, 2176, 2341, 2512], "type": "scatter", "uid": "acfc0a68-30da-11e9-9bdc-88e9fe4c5d44"}], {"title": "3x^2 - 11"}, {"showLink": true, "linkText": "Export to plot.ly"})});</script>


### Moving to derivatives of linear functions

Let's start with a function, $f(x) = 4x + 15$.  We represent the function as the following:


```python
four_x_plus_fifteen = [(4, 1), (15, 0)]
```

We can plot the function by calculating outputs at a range of x values.  Note that we use our `output_at` function to calculate the output at each individual x value.


```python
import plotly
from plotly.offline import iplot, init_notebook_mode
init_notebook_mode(connected=True)

from graph import plot, trace_values, build_layout

x_values = list(range(0, 6))
# layout = build_layout(y_axis = {'range': [0, 35]})


four_x_plus_fifteen_values = list(map(lambda x: output_at(four_x_plus_fifteen, x),x_values))
four_x_plus_fifteen_trace = trace_values(x_values, four_x_plus_fifteen_values, mode = 'lines')
plot([four_x_plus_fifteen_trace])
```


<script>requirejs.config({paths: { 'plotly': ['https://cdn.plot.ly/plotly-latest.min']},});if(!window.Plotly) {{require(['plotly'],function(plotly) {window.Plotly=plotly;});}}</script>



<div id="68a0754a-c764-4a87-ac8a-88c9b314bd24" style="height: 525px; width: 100%;" class="plotly-graph-div"></div><script type="text/javascript">require(["plotly"], function(Plotly) { window.PLOTLYENV=window.PLOTLYENV || {};window.PLOTLYENV.BASE_URL="https://plot.ly";Plotly.newPlot("68a0754a-c764-4a87-ac8a-88c9b314bd24", [{"mode": "lines", "name": "data", "text": [], "x": [0, 1, 2, 3, 4, 5], "y": [15, 19, 23, 27, 31, 35], "type": "scatter", "uid": "ad070e1a-30da-11e9-a83f-88e9fe4c5d44"}], {}, {"showLink": true, "linkText": "Export to plot.ly"})});</script>


Ok, time for what we are here for, derivatives.  Remember that the derivative is the instantaneous rate of change of a function, and is expressed as:

$$ f'(x) = \frac{\Delta f}{\Delta x}  = \frac{f(x + \Delta x) - f(x)}{\Delta x}  $$ 

#### Writing a function for $\Delta f$

We can see from the formula above that  $\Delta f = f(x + \Delta x ) - f(x) $.  Write a function called `delta_f` that, given a `list_of_terms`, an `x_value`, and a value $\Delta x $, returns the change in the output over that period.
> **Hint** Don't forget about the `output_at` function.  The `output_at` function takes a list of terms and an $x$ value and returns the corresponding output.  So really **`output_at` is equivalent to $f(x)$**, provided a function and a value of x.


```python
four_x_plus_fifteen = [(4, 1), (15, 0)]
```


```python
def delta_f(list_of_terms, x_value, delta_x):
    pass
```


```python
delta_f(four_x_plus_fifteen, 2, 1) # 4
```




    4



So for $f(x) = 4x + 15$, when x = 2, and $\Delta x = 1$, $\Delta f$ is 4.  

#### Plotting our function, delta f, and delta x  

Let's show $\Delta f$ and $\Delta x$ graphically.


```python
def delta_f_trace(list_of_terms, x_value, delta_x):
    initial_f = output_at(list_of_terms, x_value)
    delta_y = delta_f(list_of_terms, x_value, delta_x)
    trace =  trace_values(x_values=[x_value + delta_x, x_value + delta_x], 
                          y_values=[initial_f, initial_f + delta_y], mode = 'lines',
                          name = 'delta f = ' + str(delta_y))
    return trace
```


```python
trace_delta_f_four_x_plus_fifteen = delta_f_trace(four_x_plus_fifteen, 2, 1)
```

Let's add another function that shows the delta x.


```python
def delta_x_trace(list_of_terms, x_value, delta_x):
    pass
```


```python
from graph import plot, trace_values

trace_delta_x_four_x_plus_fifteen = delta_x_trace(four_x_plus_fifteen, 2, 1)
plot([four_x_plus_fifteen_trace, trace_delta_f_four_x_plus_fifteen, trace_delta_x_four_x_plus_fifteen], {'title': '4x + 15'})
```


<div id="3a393b27-e654-4b0f-ae94-29883c9f4a4b" style="height: 525px; width: 100%;" class="plotly-graph-div"></div><script type="text/javascript">require(["plotly"], function(Plotly) { window.PLOTLYENV=window.PLOTLYENV || {};window.PLOTLYENV.BASE_URL="https://plot.ly";Plotly.newPlot("3a393b27-e654-4b0f-ae94-29883c9f4a4b", [{"mode": "lines", "name": "data", "text": [], "x": [0, 1, 2, 3, 4, 5], "y": [15, 19, 23, 27, 31, 35], "type": "scatter", "uid": "ad1aef02-30da-11e9-8202-88e9fe4c5d44"}, {"mode": "lines", "name": "delta f = 4", "text": [], "x": [3, 3], "y": [23, 27], "type": "scatter", "uid": "ad1af100-30da-11e9-b6c3-88e9fe4c5d44"}, {"mode": "lines", "name": "delta x = 1", "text": [], "x": [2, 3], "y": [23, 23], "type": "scatter", "uid": "ad1af218-30da-11e9-b2e0-88e9fe4c5d44"}], {"title": "4x + 15"}, {"showLink": true, "linkText": "Export to plot.ly"})});</script>


#### Calculating the derivative

Write a function, `derivative_at` that calculates $\frac{\Delta f}{\Delta x}$ when given a `list_of_terms`, an `x_value` for the value of $(x)$ the derivative is evaluated at, and `delta_x`, which represents $\Delta x$.  

Let's try this for $f(x) = 4x + 15 $.  Round the result to three decimal places.


```python
def derivative_of(list_of_terms, x_value, delta_x):
    delta = delta_f(list_of_terms, x_value, delta_x)
    return round(delta/delta_x, 3)
```


```python
derivative_of(four_x_plus_fifteen, 3, 2) # 4.0
```




    4.0



### We do: Building more plots

Ok, now that we have written a Python function that allows us to plot our list of terms, we can write a function that called `derivative_trace` that shows the rate of change, or slope, for the function between initial x and initial x plus delta x. We'll walk you through this one.  


```python
def derivative_trace(list_of_terms, x_value, line_length = 4, delta_x = .01):
    derivative_at = derivative_of(list_of_terms, x_value, delta_x)
    y = output_at(list_of_terms, x_value)
    x_minus = x_value - line_length/2
    x_plus = x_value + line_length/2
    y_minus = y - derivative_at * line_length/2
    y_plus = y + derivative_at * line_length/2
    return trace_values([x_minus, x_value, x_plus],[y_minus, y, y_plus], name = "f' (x) = " + str(derivative_at), mode = 'lines')
```

> Our `derivative_trace` function takes as arguments `list_of_terms`, `x_value`, which is where our line should be tangent to our function, `line_length` as the length of our tangent line, and `delta_x` which is our $\Delta x$.

> The return value of `derivative_trace` is a dictionary that represents tangent line at that values of $x$.  It uses the `derivative_of` function you wrote above to calculate the slope of the tangent line.  Once the slope of the tangent is calculated, we stretch out this tangent line by the `line_length` provided.  The beginning x value is just the midpoint minus the `line_length/2` and the ending $x$ value is midpoint plus the `line_length/2`.  Then we calculate our $y$ endpoints by starting at the $y$ along the function, and having them ending at `line_length/2*slope` in either direction. 


```python
tangent_line_four_x_plus_fifteen = derivative_trace(four_x_plus_fifteen, 2, line_length = 4, delta_x = .01)
tangent_line_four_x_plus_fifteen
```




    {'mode': 'lines',
     'name': "f' (x) = 4.0",
     'text': [],
     'x': [0.0, 2, 4.0],
     'y': [15.0, 23, 31.0]}



Now we provide a function that simply returns all three of these traces.


```python
def delta_traces(list_of_terms, x_value, line_length = 4, delta_x = .01):
    tangent = derivative_trace(list_of_terms, x_value, line_length, delta_x)
    delta_f_line = delta_f_trace(list_of_terms, x_value, delta_x)
    delta_x_line = delta_x_trace(list_of_terms, x_value, delta_x)
    return [tangent, delta_f_line, delta_x_line]
```

Below we can plot our trace of the function as well 


```python
delta_x = 1

# derivative_traces(list_of_terms, x_value, line_length = 4, delta_x = .01)
three_x_plus_tangents = delta_traces(four_x_plus_fifteen, 2, line_length= 2*1, delta_x = delta_x)
# {'x': [1.5, 2, 2.5], 'y': [2.0, 4, 6.0]}
plot([four_x_plus_fifteen_trace, *three_x_plus_tangents])

```


<div id="2f23d9c6-e73a-490f-a8c3-b9d34d958fc0" style="height: 525px; width: 100%;" class="plotly-graph-div"></div><script type="text/javascript">require(["plotly"], function(Plotly) { window.PLOTLYENV=window.PLOTLYENV || {};window.PLOTLYENV.BASE_URL="https://plot.ly";Plotly.newPlot("2f23d9c6-e73a-490f-a8c3-b9d34d958fc0", [{"mode": "lines", "name": "data", "text": [], "x": [0, 1, 2, 3, 4, 5], "y": [15, 19, 23, 27, 31, 35], "type": "scatter", "uid": "ad2faca8-30da-11e9-bb46-88e9fe4c5d44"}, {"mode": "lines", "name": "f' (x) = 4.0", "text": [], "x": [1.0, 2, 3.0], "y": [19.0, 23, 27.0], "type": "scatter", "uid": "ad2faeb0-30da-11e9-a6ea-88e9fe4c5d44"}, {"mode": "lines", "name": "delta f = 4", "text": [], "x": [3, 3], "y": [23, 27], "type": "scatter", "uid": "ad2fafdc-30da-11e9-83b4-88e9fe4c5d44"}, {"mode": "lines", "name": "delta x = 1", "text": [], "x": [2, 3], "y": [23, 23], "type": "scatter", "uid": "ad2fb0d8-30da-11e9-a05e-88e9fe4c5d44"}], {}, {"showLink": true, "linkText": "Export to plot.ly"})});</script>


So that function highlights the rate of change is moving at precisely the point x = 2.  Sometimes it is useful to see how the derivative is changing across all x values.  With linear functions we know that our function is always changing by the same rate, and therefore the rate of change is constant.  Let's write a function that allows us to see the function, and the derivative side by side.


```python
from graph import make_subplots, trace_values, plot_figure

def function_derivative_compared_trace(list_of_terms, x_values, delta_x):
    function_values = list(map(lambda x: output_at(list_of_terms, x),x_values))
    derivative_values = list(map(lambda x: derivative_of(list_of_terms, x, delta_x), x_values))
    function_trace = trace_values(x_values, function_values, mode = 'lines')
    derivative_trace = trace_values(x_values, derivative_values, mode = 'lines')
    return make_subplots([function_trace], [derivative_trace])

comapared_four_x_plut_fifteen = function_derivative_compared_trace(four_x_plus_fifteen, list(range(0, 7)), 1)

plot_figure(comapared_four_x_plut_fifteen )
```

    This is the format of your plot grid:
    [ (1,1) x1,y1 ]  [ (1,2) x2,y2 ]
    



<div id="ea8fd0a8-c602-4838-a89b-7bf174a47763" style="height: 525px; width: 100%;" class="plotly-graph-div"></div><script type="text/javascript">require(["plotly"], function(Plotly) { window.PLOTLYENV=window.PLOTLYENV || {};window.PLOTLYENV.BASE_URL="https://plot.ly";
        Plotly.plot(
            'ea8fd0a8-c602-4838-a89b-7bf174a47763',
            [{"mode": "lines", "name": "data", "text": [], "x": [0, 1, 2, 3, 4, 5, 6], "y": [15, 19, 23, 27, 31, 35, 39], "type": "scatter", "uid": "ad3e1842-30da-11e9-af28-88e9fe4c5d44", "xaxis": "x", "yaxis": "y"}, {"mode": "lines", "name": "data", "text": [], "x": [0, 1, 2, 3, 4, 5, 6], "y": [4.0, 4.0, 4.0, 4.0, 4.0, 4.0, 4.0], "type": "scatter", "uid": "ad3eac62-30da-11e9-ab36-88e9fe4c5d44", "xaxis": "x2", "yaxis": "y2"}],
            {"xaxis": {"anchor": "y", "domain": [0.0, 0.45]}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0]}, "xaxis2": {"anchor": "y2", "domain": [0.55, 1.0]}, "yaxis2": {"anchor": "x2", "domain": [0.0, 1.0]}},
            {"showLink": true, "linkText": "Export to plot.ly"}
        ).then(function () {return Plotly.addFrames('ea8fd0a8-c602-4838-a89b-7bf174a47763',{});}).then(function(){Plotly.animate('ea8fd0a8-c602-4838-a89b-7bf174a47763');})
        });</script>


### Summary

In this section, we coded out our function for calculating and plotting the derivative.  We started with seeing how we can represent different types of functions.  Then we moved onto writing the `output_at` function which evaluates a provided function at a value of x.  We calculated `delta_f` by subtracting the output at initial x value from the output at that initial x plus delta x.  After calculating `delta_f`, we moved onto our `derivative_at` function, which simply divided `delta_f` from `delta_x`.  

In the final section, we introduced some new functions, `delta_f_trace` and `delta_x_trace` that plot our deltas on the graph.  Then we introduced the `derivative_trace` function that shows the rate of change, or slope, for the function between initial x and initial x plus delta x.
