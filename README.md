# K-Means

One of the main (and simplier) algorithms used in data statistics analysis is known as the k-means unsupervised clustering algorithm.  
The objective to a clustering algorithm is, given a group of n objects (or observations), partition them into k subsets (or categories) which assemble the objects sharing some properties.  
Particularly, the clustering algorithm k-means partitions n observations into k clusters (groups), where every observation belongs to the group where the centroid is the nearest.  
The Project consists in Common Lisp, Prolog and C libraries implementing Lloyd's k-means algorithm.


Lloyd's __k-means algorithm__: pseudo-code
```ruby
1: KM(n observations, k) → k clusters
2:     cs ← Initialize(k)
3:     clusters ← {}
4:     clusters0 ← Partition(observations, cs)
5:     if clusters = clusters0 then
6:         return clusters
7:     else
8:         clusters ← clusters0
9:         cs ← RecomputeCentroids(clusters)
10:        goto 3
11:    end if
```


## List of contents

- [Interface](#interface)
- [Examples](#examples)


## Interface

### C

__km__(_double ** observations_, _int k_, _int observations size_, _int vector size_) → _double *** clusters_  

__centroid__(_double ** observations_, _int observations size_, _int vector size_) → _double * centroid_  

__vsum__(_double * vector1_, _double * vector2_, _int vector size_) → _double * vsum_  

__vsub__(_double * vector1_, _double * vector2_, _int vector size_) → _double * vsub_  

__innerprod__(_double * vector1_, _double * vector2_, _int vector size_) → _double innerprod_  

__norm__(_double * vector_, _int vector size_) → _double norm_  

__print_vector__(_double * vector_, _int vector size_) → _void_   

__print_observations__(_double ** observations_, _int observations size_, _int vector size_) → _void_  

__print_clusters__(_double *** clusters_, _int k_, _int observations size_, _int vector size_) → _void_  


### Common Lisp

(__km__ _observations k_) → _clusters_  

(__centroid__ _observations_) → _centroid_  

(__vsum__ _vector1 vector2_) → _v_  

(__vsub__ _vector1 vector2_) → _v_  

(__innerprod__ _vector1 vector2_) → _v_  

(__norm__ _vector_) → _v_  


### Prolog

__km__(_+Observations_, _+K_, _-Clusters_)  

__centroid__(_+Observations_, _-Centroid_)  

__vsum__(_+Vector1_, _+Vector2_, _-V_)  

__vsub__(_+Vector1_, _+Vector2_, _-V_)  

__innerprod__(_+Vector1_, _+Vector2_, _-R_)  

__norm__(+_Vector_, _-N_)  

__new_vector__(_+Name_, _+Vector_)  


### Ruby

__km__(_observations[][]_, _k_) → _clusters[][][]_  

__centroid__(_observations[][]_) → _centroid[]_  

__vsum__(_vector1[]_, _vector2[]_) → _vsum[]_  

__vsub__(_vector1[]_, _vector2[]_) → _vsub[]_  

__innerprod__(_vector1[]_, _vector2[]_) → _innerprod_  

__norm__(_vector[]_) → _norm_


## Examples

Consider the set of 2D Observations:

```
O = {(3.0, 7.0), (0.5, 1.0), (0.8, 0.5), (1.0, 8.0),
     (0.9, 1.2), (6.0, 4.0), (7.0, 5.5),
     (4.0, 9.0), (9.0, 4.0)}.
```

The three clusters (with k = 3) calculated with the K-Means algorithm are:

```
1. {(1.0, 8.0), (3.0, 7.0), (4.0, 9.0)},
2. {(0.5, 1.0), (0.8, 0.5), (0.9, 1.2)},
3. {(6.0, 4.0), (7.0, 5.5), (9.0, 4.0)}.
```

### C

```c
int observations_size = 9;
int vector_size = 2;
int k = 3;

print_observations(observations, observations_size, vector_size);

double *** clusters = km(observations, k, observations_size, vector_size);
print_clusters(clusters, k, observations_size, vector_size);
```

Output:

```
[(3.000000, 7.000000), (0.500000, 1.000000), (0.800000, 0.500000),
 (1.000000, 8.000000), (0.900000, 1.200000), (6.000000, 4.000000),
 (7.000000, 5.500000), (4.000000, 9.000000), (9.000000, 4.000000)]

{[(0.500000, 1.000000), (0.800000, 0.500000), (0.900000, 1.200000)],
 [(6.000000, 4.000000), (7.000000, 5.500000), (9.000000, 4.000000)],
 [(3.000000, 7.000000), (1.000000, 8.000000), (4.000000, 9.000000)]}
```


### Common Lisp

```lisp
CL prompt> (defparameter v3 (list 1 2 3))
V3

CL prompt> v3
(1 2 3)

CL prompt> (srqt (innerprod V3 V3))
3.7416575 ; Il risultato può variare.

CL prompt> (norm V3)
3.7416575

CL prompt> (vsum V3 (list 10 0 42))
(11 2 45)

CL prompt> (defparameter observations
                         ’((3.0 7.0) (0.5 1.0) (0.8 0.5)
                           (1.0 8.0) (0.9 1.2) (6.0 4.0)
                           (7.0 5.5) (4.0 9.0) (9.0 4.0)))

CL prompt> (km observations 3)
(((1.0 8.0) (3.0 7.0) (4.0 9.0))
 ((0.5 1.0) (0.8 0.5) (0.9 1.2))
 ((6.0 4.0) (7.0 5.5) (9.0 4.0)))
```


### Prolog

```prolog
?- new_vector(v3, [1, 2, 3]).
true.

?- vector(v3, V).
V = [1, 2, 3].

?- vector(v3, V), innerprod(V, V, IP), N is sqrt(IP).
V = [1, 2, 3].
N = 3.7416575.

?- vector(v3, V), norm(V, N).
V = [1, 2, 3].
N = 3.7416575.

?- vector(v3, V), vsum(V, [10, 0, 42], S).
V = [1, 2, 3].
S = [11, 2, 45].

?- km([[3.0, 7.0], [0.5, 1.0], [0.8, 0.5],
       [1.0, 8.0], [0.9, 1.2], [6.0, 4.0],
       [7.0, 5.5], [4.0, 9.0], [9.0, 4.0]],
      3,
      Clusters).
Clusters = [[[1.0, 8.0], [3.0, 7.0], [4.0, 9.0]],
            [[0.5, 1.0], [0.8, 0.5], [0.9, 1.2]]
            [[6.0, 4.0], [7.0, 5.5], [9.0, 4.0]]].
```


### Ruby

```ruby
[1] pry(main)> observations = [[3.0, 7.0], [0.5, 1.0], [0.8, 0.5],
                               [1.0, 8.0], [0.9, 1.2], [6.0, 4.0],
                               [7.0, 5.5], [4.0, 9.0], [9.0, 4.0]]
=> [[3.0, 7.0], [0.5, 1.0], [0.8, 0.5],
    [1.0, 8.0], [0.9, 1.2], [6.0, 4.0],
	[7.0, 5.5], [4.0, 9.0], [9.0, 4.0]]

[2] pry(main)> k = 3
=> 3

[3] pry(main)> km(observations, k)
=> [[[0.5, 1.0], [0.8, 0.5], [0.9, 1.2]],
 	[[6.0, 4.0], [7.0, 5.5], [9.0, 4.0]],
	[[3.0, 7.0], [1.0, 8.0], [4.0, 9.0]]]
```
