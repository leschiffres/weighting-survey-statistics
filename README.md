# Simplified Weighting Algorithm for Survey statistics

The purpose of this repository is to use a simplified version of the [raking algorithm](https://en.wikipedia.org/wiki/Raking), to showcase its capabilities.

By making use of the titanic dataset, we create a biased sample where there is a slight deviation of the initial distribution. Initially we have the following distribution:

```python
'Pclass': {3: 0.542, 1: 0.247, 2: 0.212},
'Sex': {'male': 0.644, 'female': 0.356},
'FareGroup': {'Cheap': 0.575, 'Average': 0.241, 'Above Average': 0.119, 'Expensive': 0.064}
```

and the biased sample has the following one:

```python
'Pclass': {3: 0.44, 1: 0.296, 2: 0.264},
'Sex': {'male': 0.568, 'female': 0.432},
'FareGroup': {'Cheap': 0.512,'Average': 0.252,'Above Average': 0.16,'Expensive': 0.076}
```

Clearly in the biased sample there's a higher than expected percentage of female passengers and a lower percentage of the 3rd class passengers.

Normally in survey statistics we cannot know the actual distribution of all opinion data / population characteristics, so we try to estimate the based on the things we know. Thus, if we have a survey where 70% of the voters were male, we try to balance it out as we know that normally the percentage of men and women in the general population, should be closer to 50%. This is when a weighting algorithm can help.

Here we assume that the `Pclass` and `Sex` population data are known and we try to estimate the `FareGroup` one.

We implement a simple algorithm that:

- In the beginning each row gets a weight equal to 1 and then we adjust it accordingly.
    - For instance if we know that the actual male percentage is 65% but in our sample men are at 32.5% then every male row should get a doubled weight.
- We divide the actual ratio by the observed ratio for each category to get the raking factor.
- Then each row gets their weight multiplied by the corresponding factor of `Pclass` and `Sex` to get the final weight.

Thus, we are able to get a final estimation of the `FareGroup` distribution.
```
Cheap            0.588861
Average          0.220911
Above Average    0.134534
Expensive        0.060532
```

Clearly, the this is much closer to the reality that what is observed in the biased sample.
