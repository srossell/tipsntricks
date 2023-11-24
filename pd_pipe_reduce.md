# functools.reduce to repeatedly pipe a function with pandas

Based on this [so answer](https://stackoverflow.com/a/68652715])

Suppose you start with a single column dataframe, and you want to create columns that add different numbers to that first column.

Step by step you would do the following:

```python
# Setup the dataframe
import pandas as pd

df = pd.DataFrame(np.ones((4, 1), float), columns=["start"])
```

![image](https://github.com/srossell/tipsntricks/assets/7842572/96b5c3eb-388b-4379-95c4-74efc9cb7d41)

```python
# a function to use with pandas.pipe
def add_in_new_col(df, firstcol, num2add, newcol):
    df.loc[:, newcol] = df.loc[:, firstcol] + num2add
    return df

step_by_step = (
    df
    .pipe(add_in_new_col, "start", 2, "plus2")
    .pipe(add_in_new_col, "start", 5, "plus5")
)
```
![image](https://github.com/srossell/tipsntricks/assets/7842572/8d92f62c-65c9-44ab-a5a4-ee028992e5e1)

Now using reduce ``` reduce(function, iterable[, initial]) -> value ```
```python
from functools import reduce

myiterable = [
    ("start", 3, "plus3"),
    ("start", 4, "plus4")
]

with_reduce = reduce(lambda _df, myargs: _df.pipe(add_in_new_col, *myargs), myiterable, df)
```
![image](https://github.com/srossell/tipsntricks/assets/7842572/80342424-a330-4e08-ad03-cb5e62cb31e0)
