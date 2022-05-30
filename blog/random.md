---
layout: default
title:  "Random Number Generators' Performance"
parent: Blog
nav_order: 2
---

## Random Number Generators' Performance

I have done a speed test about how the random number generators perform on my MacBook Air(M1, 2020, 16GB memory). Below is the code snippet I used to count the time for different random number generators.

```cpp
    #include <random>
    
    std::mt19937 rng{42};
    size_t num = 1000000000uLL;
    unsigned int eps;
    for (size_t i = 0; i < num; ++i) eps = rng();
```

|Random number generator | Time used (s)|
|:------------------------|:---:|
|std::minstd_rand | 4.093|
|std::mt_19937 | 3.162|
|std::mt_19937_64 |3.150|
|boost::minstd_rand|4.091|
|boost::mt19937|0.528|
|boost::mt19937_64|0.845|
|boost::mt11213b|0.551|
|boost::taus88|1.117|
