[![Coverage Status](https://coveralls.io/repos/matthieugomez/FixedEffects.jl/badge.svg?branch=master)](https://coveralls.io/r/matthieugomez/FixedEffects.jl?branch=master)
[![Build Status](https://travis-ci.org/matthieugomez/FixedEffects.jl.svg?branch=master)](https://travis-ci.org/matthieugomez/FixedEffects.jl)

The package `fixedeffects` estimates models with high dimensional fixed effects. It is a basic and mostly untested implementation of the packages `reghdfe` in Stata and `lfe` in R


## demean
The function `demean` demeans columns with respect to fixed effects. 

```julia
using DataArrays, DataFrame, RDataSets, FixedEffects
df = dataset("plm", "Cigar")
df[:State] = PooledDataArray(df[:State])
df[:Year] = PooledDataArray(df[:Year])
demean(df, [:Sales], nothing ~ State + Year)
```

`demean!` replaces columns in the original dataset


Add interactions with continuous variable with `&`
```julia
df = dataset("plm", "Cigar")
df[:State] = PooledDataArray(df[:State])
demean(df, [:Sales], nothing ~ State + State&Year)
```

Construct one fixed effect from a set of variables using `group`

```julia
df[:StateYear] = group(df[:State, :Year])
demean(df, [:Sales], nothing ~ StateYear)
```



## areg
The function `areg` simply estimates a linear model after demeaning variables. In particular errors are not adjusted for dof etc.

```julia
areg(Sales~NDI, df, nothing ~ State + Year)
```



