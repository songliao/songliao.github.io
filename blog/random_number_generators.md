---
layout: default
title:  "Random Number Generators' Performance"
parent: Blog
nav_order: 2
---

## Random Number Generators' Performance

I have done a speed test about how the random number generators perform on my MacBook Air(M1, 2020, 16GB memory). Below is the code snippet I used to count the time for different random number generators.

The random number generators are from [C++ standard library](https://en.cppreference.com/w/cpp/numeric/random), the [Boost Random library](https://www.boost.org/doc/libs/1_79_0/doc/html/boost_random/reference.html#boost_random.reference.concepts) and random number generator recommended by the [Numerical Recipes, 3rd edition](http://numerical.recipes).

```cpp
    #include <random>
    
    std::mt19937 rng{42};
    size_t num = 1000000000uLL;
    unsigned int eps;
    for (size_t i = 0; i < num; ++i) eps = rng();
```

|Random number generator | Time used (s)|
|:------------------------|:---|
|std::minstd_rand | 4.093|
|std::mt19937 | 3.162|
|std::mt19937_64 |3.150|
|boost::random::minstd_rand|4.091|
|boost::random::mt19937|0.528|
|boost::random::mt19937_64|0.845|
|boost::random::mt11213b|0.551|
|boost::random::taus88|1.117|
|NR3::Ran|2.233|

As in most pratical cases, the speed of generating normally distributed random numbers is of great importance. I have also done a speed test on that using boost::mt19937.

According to the [official document](https://www.boost.org/doc/libs/1_79_0/doc/html/boost/random/normal_distribution.html), Boost.Random uses the [Ziggurat algorithm](https://en.wikipedia.org/wiki/Ziggurat_algorithm).

The C++ standard library implements the [Box-Muller algorithm](https://en.wikipedia.org/wiki/Box–Muller_transform) in the Polar form.

```cpp
    #include <boost/random/normal_distribution.hpp>
    #include <boost/random/mersenne_twister.hpp>
    
    boost::random::mt19937 rng{42};
    boost::random::normal_distribution<double> dist(0, 1);
    size_t num = 1000000000uLL;
    unsigned int eps;
    for (size_t i = 0; i < num; ++i) eps = dist(rng);
```

It costs about 6.311s (averaged from 5 experiments).

Similarily, I did serveral test by combining the random number generator and the distribution transform algorithm. The results are shown below

|random number generator | distribution| Time used (s)|
|:------------------------|:---|--|
|std::mt19937 | std::normal_distribution|17.683|
|std::mt19937| boost::random::normal_distribution|10.767|
|boost::random::mt19937|std::normal_distribution|10.483|
|boost::random::mt19937|boost::random::normal_distribution|6.439|
