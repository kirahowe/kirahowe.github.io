Title: Data Manipulation in Clojure Compared to R and Python
Date: 2024-07-18
Tags: clojure, tools, scicloj, r, python

I spend a lot of time developing and teaching people about Clojure's open source tools for working with data. Almost everybody who wants to use Clojure for this kind of work is coming from another language ecosystem, usually R or Python. Together with Daniel Slutsky, I'm working on formalizing some of the common teachings into a course. Part of that is providing context for people coming from other ecosystems, including "translations" of how to accomplish data science tasks in Clojure.

As part of this development, I wanted to share an early preview in this blog post. The format is inspired by this great blog post I read a while ago comparing [R and Polars](https://krz.github.io/Comparing-dplyr-with-polars/) side by side (where "R" here refers to [the tidyverse](https://www.tidyverse.org), an opinionated collection of R libraries for data science, and realistically mostly [`dplyr`](https://dplyr.tidyverse.org) specifically). I'm adding Pandas because it's among the most popular dataset manipulation libraries, and of course Clojure, specifically [tablecloth](https://github.com/scicloj/tablecloth), the primary data manipulation library in our ecosystem.

I'll use the same dataset as the original blog post, the [Palmer Penguin dataset](https://allisonhorst.github.io/palmerpenguins/articles/intro.html). For the sake of simplicity, I saved a copy of the dataset as a CSV file and made it available on this website. I will also refer the data as a "dataset" throughout this post because that's what Clojure people call a tabular, column-major data structure, but it's the same thing that is variously referred to as a dataframe, data table, or just "data" in other languages. I'm also assuming you know how to install the packages required in the given ecosystems, but any necessary imports or requirements are included in the code snippets the first time they appear. Versions of all languages and libraries used in this post are listed at the end. Here we go!

## Reading data
Reading data is straightforward in every language, but as a bonus we want to be able to indicate on the fly which values should be interpreted as "missing", whatever that means in the given libraries. In this dataset, the string `"NA"` means "missing", so we want to tell the dataset constructor this as soon as possible. Here's the comparison of how to accomplish that in various languages:

### Tablecloth
```clojure
(require '[tablecloth.api :as tc])

(def ds 
  (tc/dataset "https://codewithkira.com/assets/penguins.csv"))
```

Note that tablecloth interprets the string "NA" as missing (`nil`, in Clojure) by default.

### R
In reality, in R you would get the dataset from the R [package that contains the dataset](https://allisonhorst.github.io/palmerpenguins/articles/intro.html). This is a fairly common practice in R. In order to compare apples to apples, though, here I'll show how to initialize the dataset from a remote CSV file, using the [`readr` package's `read_csv`](https://readr.tidyverse.org/reference/read_delim.html), which is part of the tidyverse:

```r
library(tidyverse)

ds <- read_csv("https://codewithkira.com/assets/penguins.csv",
               na = "NA")
```
### Pandas
```python
import pandas as pd

ds = pd.read_csv("https://codewithkira.com/assets/penguins.csv")
```

Note that pandas has a [fairly long list](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html) of values it considers `NaN` already, so we don't need to specify what missing values look like in our case, since `"NA"` is already in that list.

### Polars
```python
import polars as pl

ds = pl.read_csv("https://codewithkira.com/assets/penguins.csv",
                 null_values="NA")
```


## Basic commands to explore the dataset
The first thing people usually want to do with their dataset is see it and poke around a bit. Below is a comparison of how to accomplish basic data exploration tasks using each library.

| Operation               | tablecloth                                 | dplyr                     |
| ----------------------- | ------------------------------------------ | ------------------------- |
| see first 10 rows       | `(tc/head ds 10)`                          | `head(ds, 10)`            |
| see all column names    | `(tc/column-names ds)`                     | `colnames(ds)`            |
| select column           | `(tc/select-columns ds "year")`            | `select(ds, year)`        |
| select multiple columns | `(tc/select-columns ds ["year" "sex"])`    | `select(ds, year, sex)`   |
| select rows             | `(tc/select-rows ds #(> (% "year") 2008))` | `filter(ds, year > 2008)` |
| sort column             | `(tc/order-by ds "year")`                      | `arrange(ds, year)`       |

<br>

| Operation               | pandas                   | polars                             |
| ----------------------- | ------------------------ | ---------------------------------- |
| see first `n` rows      | `ds.head(10)`            | `ds.head(10)`                      |
| see all column names    | `ds.columns`             | `ds.columns`                       |
| select column           | `ds[["year"]]`           | `ds.select(pl.col("year"))`        |
| select multiple columns | `ds[["year", "sex"]]`    | `ds.select(pl.col("year", "sex"))` |
| select rows             | `ds[ds["year"] > 2008]`  | `ds.filter(pl.col("year") > 2008)` |
| sort column             | `ds.sort_values("year")` | `ds.sort("year")`                  |


Note there are some differences in how different libraries sort missing values, for example in tablecloth and polars they are placed at the beginning (so they're at the top when a column is sorted in ascending order and last when descending), but dplyr and pandas place them last (regardless of whether ascending or descending order is specified).

As you can see, these commands are all pretty similar, with the exception of selecting rows in tablecloth. This is a short-hand syntax for writing an anonymous function in Clojure, which is how rows are selected. Being a functional language, functions in Clojure are "first-class", which basically just means they are passed around as arguments willy-nilly, all over the place, all the time. In this case, the third argument to tablecloth's `select-rows` function is a predicate (a function that returns a boolean) that takes as its argument a dataset row as a map of column names to values. Don't worry, though, tablecloth doesn't process your entire dataset row-wise. Under the hood datasets are highly optimized to perform column-wise operations as fast as possible.

Here's an example of what it looks like to string a couple of these basic dataset exploration operations together, for example in this case to get the `bill_length_mm` of all penguins with `body_mass_g` below 3800:

### Tablecloth
```clojure
(-> ds
    (tc/select-rows #(and (% "body_mass_g")
                          (> (% "body_mass_g") 3800)))
    (tc/select-columns "bill_length_mm"))
```
Note that in tablecloth we have to explicitly omit rows where the value we're filtering by is missing, unlike in other libraries. This is because tablecloth actually uses `nil` (as opposed to a library-specific construct) to indicate a missing value , and in Clojure `nil` is not treated as comparable to numbers. If we were to try to compare `nil` to a number, we would get an exception telling us that we're trying to compare incomparable types. Clojure is fundamentally dynamically typed in that it only does type checking at runtime and bindings can refer to values of any type, but it is also strongly typed, as we see here, in the sense that it explicitly avoids implicit type coercion. For example deciding whether 0 is greater or larger than `nil` requires some assumptions, and these are intentionally not baked into the core of Clojure or into tablecloth as a library as is the case in some other languages and libraries.

This example also introduces Clojure's "thread-first" macro. The `->` arrow is like R's `|>` operator or the unix pipe, effectively passing the output of each function in the chain as input to the next. It comes in very handy for data processing code like this.

Here is the equivalent operation in the other libraries:

### dplyr
```r
ds |>
    filter(body_mass_g < 3800) |>
    select(bill_length_mm)
```

### Pandas
```python
ds[ds["body_mass_g"] < 3800]["bill_length_mm"]
```

### Polars
```python
ds.filter(pl.col("body_mass_g") < 3800).select(pl.col("bill_length_mm"))
```

## More advanced filtering and selecting
Here is what some more complicated data wrangling looks like across the libraries.

### Select all columns except for one
| Library | Code |
| --- | --- |
| tablecloth | `(tc/select-columns ds (complement #{"year"}))` |
| dplyr | `select(ds, -year)` |
| pandas | `ds.drop(columns=["year"])` |
| polars | `ds.select(pl.exclude("year"))` |

Another property of functional languages in general, and especially Clojure, is that they really take advantage of the fact that a lot of things are functions that you might not be used to treating like functions. They also leverage function composition to simply combine multiple functions into a single operation.

For example a set (indicated with the `#{}` syntax in Clojure) is a special function that returns a boolean indicating whether the given argument is a member of the set or not. And [`complement`](https://clojuredocs.org/clojure.core/complement) is a function in `clojure.core` that effectively inverts the function given to it, so combined `(complement #{"year"})` means "every value that is _not_ in the set `#{"year"}`, which we can then use as our predicate column selector function to filter out certain columns.

### Select all columns that start with a given string
| Library | Code |
| --- | --- |
| tablecloth | `(tc/select-columns ds #(str/starts-with? % "bill"))` |
| dplyr | `select(ds, starts_with("bill"))` |
| pandas | `ds.filter(regex="^bill")` |
| polars | <pre style="width:max-content"><code>import polars.selectors as cs</code><br><code>ds.select(cs.starts_with("bill"))</code></pre> |

### Select only numeric columns
| Library | Code |
| --- | --- |
| tablecloth | `(tc/select-columns ds :type/numerical)` |
| dplyr | `select(ds, where(is.numeric))` |
| pandas | `ds.select_dtypes(include='number')` |
| polars | `ds.select(cs.numeric())` |

The symbol `:type/numerical` in Clojure here is a magic keyword that tablecloth knows about and can accept as a column selector. This list of magic keywords that tablecloth knows about is not (yet) documented anywhere, but it is [available in the source code](https://github.com/scicloj/tablecloth/blob/b0faadcd202d4355767f7e212a4d86e099eb5f96/src/tablecloth/api/utils.clj#L59).

### Filter rows for range of values
| Library | Code |
| --- | --- |
| tablecloth | `(tc/select-rows ds #(< 3500 (% "body_mass_g" 0) 4000))` |
| dplyr | `filter(ds, between(body_mass_g, 3500, 4000))` |
| pandas | `ds[ds["body_mass_g"].between(3500, 4000)]` |
| polars | `ds.filter(pl.col("body_mass_g").is_between(3500, 4000))` |

Note here we handle the missing values in the `body_mass_g` column differently than above, by specifying a default value for the map lookup. We're explicitly telling tablecloth to treat missing values as `0` in this case, which can then be compared to other numbers. This is probably the better way to handle this case, but the method above works, too, plus it gave me the opportunity to soapbox about Clojure types for a moment.

### Reshaping the dataset
#### Tablecloth
```clojure
(tc/pivot->longer ds 
                  ["bill_length_mm" "bill_depth_mm"
                   "flipper_length_mm" "body_mass_g"]
                  {:target-columns "measurement" :value-column-name "value"})
```

#### dplyr
```r
ds |>
    pivot_longer(cols = c(bill_length_mm, bill_depth_mm,
                          flipper_length_mm, body_mass_g),
                 names_to = "measurement",
                 values_to = "value")
```

#### Pandas
```python
pd.melt(
    ds, 
    id_vars=ds.columns.drop(["bill_length_mm", "bill_depth_mm", 
                             "flipper_length_mm", "body_mass_g"]), 
    var_name="measurement",
    value_name="value"
)
```

#### Polars
```python
ds.unpivot(
     index=set(ds.columns) - set(["bill_length_mm",
                                  "bill_depth_mm",
                                  "flipper_length_mm",
                                  "body_mass_g"]),
     variable_name="measurement",
     value_name="value")
```

## Creating and renaming columns
### Adding columns based on some other existing columns
There are many reasons you might want to add columns, and often new columns are combinations of other ones. Here's how you'd generate a new column based on the values in some other columns in each library:

| Library | Code |
| --- | --- |
| tablecloth | <pre style="width:max-content"><code>(require '[tablecloth.column.api :as tcc])<br>(tc/add-columns ds {"ratio" (tcc// (ds "bill\_length\_mm")<br>                                   (ds "flipper\_length\_mm"))})</code></pre> |
| dplyr | `mutate(ds, ratio = bill_length_mm / flipper_length_mm)` |
| pandas | `ds["ratio"] = ds["bill_length_mm"] / ds["flipper_length_mm"]` |
| polars | <pre style="width:max-content"><code>ds.with\_columns(<br>    (pl.col("bill\_length\_mm") /<br>     pl.col("flipper\_length\_mm")).alias("ratio")<br>)</code></pre> |

Note that this is where the wheels start to come off if you're not working in a functional way with immutable data structures. Clojure data structures (including tablecloth datasets) are immutable, which is not the case Pandas. The Pandas code above mutates the dataset in place, so as soon as you do any mutating operations like these, you now have to keep mental track of the state of your dataset, which can quickly lead to high cognitive overhead and lots of incidental complexity.

### Renaming columns
| Library | Code |
| --- | --- |
| tablecloth | `(tc/rename-columns ds {"bill_length_mm" "bill_length"})` |
| dplyr | `rename(ds, bill_length = bill_length_mm)` |
| pandas | `ds.rename(columns={"bill_length_mm": "bill_length"})` |
| polars | `ds.rename({"bill_length_mm": "bill_length"})` |

Again beware, the Pandas implementation shown here mutates the dataset in place. Also manually specifying every column name transformation you want to do is one way to accomplish the task, but sometimes that can be tedious if you want to apply the same transformation to every column name, which is fairly common.

### Transforming column names
Here's how you would upper case all column names:
| Library | Code |
| --- | --- |
| tablecloth | `(tc/rename-columns ds :all str/upper-case)` |
| dplyr | `rename_with(ds, toupper)` |
| pandas | `ds.columns = ds.columns.str.upper()` |
| polars | `ds.select(pl.all().name.to_uppercase())` |

Like the other libraries, tablecloth's `rename-columns` accepts both types of arguments -- a simple mapping of old -> new column names, or any column selector and any transformation function. For example, removing the units from each column name would look like this in each language:

| Library | Code |
| --- | --- |
| tablecloth | <code>(tc/rename-columns ds #".+\_(mm&#124;g)" #(str/replace % #"(.+)\_(mm&#124;g)" "$1"))</code> |
| dplyr | <code>rename\_with(penguins, ~ str\_replace(.x, "\^(.+)\_(mm&#124;g)$", "\\1"))</code> |
| pandas | <pre style="width:max-content"><code>import re<br>ds.rename(columns=lambda x: re.sub(r"(.+)_(mm&#124;g)$", r"\1", x))</code></pre> |
| polars | <pre style="width:max-content"><code>ds = ds.rename({<br>    col: col.replace("\_mm", "").replace("\_g", "")<br>    for col in ds.columns<br>})</code></pre> |

## Grouping and aggregating

Grouping behaves [somewhat unconventionally in tablecloth](https://scicloj.github.io/tablecloth/index.html#group-by). Datasets can be grouped by a single column name or a sequence of column names like in other libraries, but grouping can also be done using any arbitrary function. Grouping in tablecloth also returns a new dataset, similar to dplyr, rather than an abstract intermediate object (as in pandas and polars). Grouped datasets have three columns, (name of the group, group id, and a column containing a new dataset of the grouped data). Once a dataset is grouped, the group values can be aggregated in a variety of ways. Here are a few examples, with comparisons between libraries:

### Summarizing counts
To get the count of each penguin by species:

#### Tablecloth
```clojure
(-> ds
    (tc/group-by ["species"])
    (tc/aggregate {"count" tc/row-count}))
```

#### dplyr
```r
ds |>
    group_by(species) |>
    summarise(count = n())
```

#### Pandas
```python
ds.groupby("species").agg(count=("species", "count"))
```

#### Polars
```python
ds.group_by("species").agg(pl.count().alias("count"))
```

### Find the penguin with the lowest body mass by species
#### Tablecloth
```clojure
(-> ds
    (tc/group-by ["species"])
    (tc/aggregate {"lowest_body_mass_g" #(->> (% "body_mass_g")
                                              tcc/drop-missing
                                              (apply tcc/min))}))
```

#### dplyr
```r
ds |>
    group_by(species) |>
    summarize(lowest_body_mass_g = min(body_mass_g, na.rm = TRUE))
```

#### Pandas
```python
ds.groupby("species").agg(
    lowest_body_mass_g=("body_mass_g", lambda x: x.min(skipna=True))
).reset_index()
```

#### Polars
```python
ds.group_by("species").agg(
    pl.col("body_mass_g").min().alias("lowest_body_mass_g")
)
```

## Conclusions
As you can see, all of these libraries are perfectly suitable for accomplishing common data manipulation tasks. Choosing a language and library can impact code readability, maintainability, and performance, though, so understanding the differences between available toolkits can help us make better choices.

Clojure's tablecloth emphasizes functional programming concepts and immutability, which can lead to more predictable and re-usable code, at the cost of adopting a potentially new paradigm. Hopefully this comparison serves not only as a translation guide, but an an intro to the different philosophies underpinning these common data science tools.

Thanks for reading :)

## Versions
The code in this post works with the following language and library versions:

| Tool | Version |
| --- | --- |
| MacOS | Sonoma 14.5 |
| JVM | `21.0.2` |
| Clojure | `1.11.1` |
| Tablecloth | `7.021` |
| R | `4.4.1` |
| Tidyverse | `2.0.0` |
| Python | `3.12.3` |
| Pandas | `2.1.4` |
| Polars | `1.1.0` |
