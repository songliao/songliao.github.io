---
layout: default
title:  "Random Number Generators' Performance"
parent: Blog
nav_order: 2
---

## Random Number Generators' Performance

I have done a set of speed test about how the random number generators perform on my MacBook Air(M1, 2020, 16GB memory) and my PC(Ubuntu 22.04 LTS, Intel(R) Core(TM) i7-8700K CPU @ 3.70GHz). Below is the code snippet I used to count the time for different random number generators.

The random number generators are from [C++ standard library](https://en.cppreference.com/w/cpp/numeric/random), and the [Boost Random library](https://www.boost.org/doc/libs/1_79_0/doc/html/boost_random/reference.html#boost_random.reference.concepts).

```cpp
    #include <random>
    
    std::mt19937 rng{42};
    size_t num = 1000000000uLL;
    unsigned int eps;
    for (size_t i = 0; i < num; ++i) eps = rng();
```

|Random number generator | Time(s) used on Mac| Time(s) used on PC|
|:------------------------|:---|:--|
|std::minstd_rand | 4.093| 3.095
|std::mt19937 | 3.162| 3.807
|std::mt19937_64 |3.150| 4.172
|boost::random::minstd_rand|4.091|3.366
|boost::random::mt19937|0.528|0.816
|boost::random::mt19937_64|0.845|1.113
|boost::random::mt11213b|0.551|0.624
|boost::random::taus88|1.117|1.389


As in most pratical cases, the speed of generating normally distributed random numbers is of great importance.

According to the [official document](https://www.boost.org/doc/libs/1_79_0/doc/html/boost/random/normal_distribution.html), Boost.Random uses the [Ziggurat algorithm](https://en.wikipedia.org/wiki/Ziggurat_algorithm). And The C++ standard library implements the [Box-Muller algorithm](https://en.wikipedia.org/wiki/Box–Muller_transform) in the Polar form.

```cpp
    #include <boost/random/normal_distribution.hpp>
    #include <boost/random/mersenne_twister.hpp>
    
    boost::random::mt19937 rng{42};
    boost::random::normal_distribution<double> dist(0, 1);
    size_t num = 1000000000uLL;
    unsigned int eps;
    for (size_t i = 0; i < num; ++i) eps = dist(rng);
```

Similarily, I did serveral test by combining the random number generator and the distribution transform algorithm. The results are shown below

|random number generator | distribution| Time(s) used on Mac| Time(s) used on PC|
|:------------------------|:---|:--|:--|
|std::mt19937 | std::normal_distribution|17.683|21.652
|std::mt19937| boost::random::normal_distribution|10.767|13.470
|boost::random::mt19937|std::normal_distribution|10.483|14.405
|boost::random::mt19937|boost::random::normal_distribution|6.439|6.780
|boost::random::mt11213b|std::normal_distribution||14.109
|boost::random::mt11213b|boost::random::normal_distribution||6.386

