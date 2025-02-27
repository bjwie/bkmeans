# The Breathing *K*-Means Algorithm

An approximation algorithm for the *k*-means problem that (on average) is **better** (higher solution quality) and **faster** (lower CPU time usage) than  ***k*-means++**. 

**Techreport:**
https://arxiv.org/abs/2006.15666 (submitted for publication)

**Repo (with examples):**
https://github.com/gittar/breathing-k-means

## API
The included class **BKMeans** is subclassed from [scikit-learn's **KMeans** class](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html)
and has, therefore, **the same API**. It can be used as a plug-in replacement for scikit-learn's **KMeans**. 

There is one new parameters which can be ignored (left at default) for normal usage:

* *m* (breathing depth), default: 5

The parameter *m* can also be used, however, to generate faster ( 1 < *m* < 5) or better (*m*>5) solutions. For details see the above techreport.


## Installation


```bash
pip install bkmeans
```
## Example 1: running on simple random data set
Code:
```python
import numpy as np
from bkmeans import BKMeans

# generate random data set
X=np.random.rand(1000,2)

# create BKMeans instance
bkm = BKMeans(n_clusters=100)

# run the algorithm
bkm.fit(X)

# print SSE (inertia in scikit-learn terms)
print(bkm.inertia_)
```
Output:
```
1.1775040547902602
```

## Example 2: comparison with *k*-means++ (multiple runs)
Code:
```python
import numpy as np
from sklearn.cluster import KMeans
from bkmeans import BKMeans

# random 2D data set
X=np.random.rand(1000,2)

# number of centroids
k=100

for i in range(5):
    # kmeans++
    kmp = KMeans(n_clusters=k)
    kmp.fit(X)

    # breathing k-means
    bkm = BKMeans(n_clusters=k)
    bkm.fit(X)

    # relative SSE improvement of bkm over km++
    imp = 1 - bkm.inertia_/kmp.inertia_
    print(f"SSE improvement over k-means++: {imp:.2%}")
```
Output:

```
SSE improvement over k-means++: 3.38%
SSE improvement over k-means++: 4.16%
SSE improvement over k-means++: 6.14%
SSE improvement over k-means++: 6.79%
SSE improvement over k-means++: 4.76%
```


