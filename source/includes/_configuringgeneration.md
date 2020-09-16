# Configuring Generation

Tryal.AI allows you a lot of control over how you generate material. We estimate that with the parameters currently available, there are 525 Sextillion combinations (525,000,000,000,000,000,000,000) of ways to configure our generator. 

In most cases these changes are inconsequential - 79 marks is after all, pretty much 80. There are certain configurations for which Tryal.AI will never work. Due however, to the vast number of ways to configure the generator, it's not necessarily possible for us to do this sense checking at runtime. 

Instead, Tryal.AI's internal mechanisms are designed to fail gracefully, when a given configuration results in a contradictory and impossible to satisfy request. However, being non-deterministic, it is also possible that Tryal.AI failed to generate material in this specific moment. Two identical requests made now are not both guaranteed to succeed.

We try to minimise failure for reasons other than bad configuration input, but it's impossible to completely reduce the risk. Additionally, more complex configurations can lower the chance of Tryal.AI succeeding. This typically occurs during requests that narrow the number of questions we can choose from (such as when limiting by topics or subtopics). 

## An example of bad configuration

> In this example we use YAML for ease of reading.

```yaml
quantity: 50
repeat: 'off'
exclude: 
  algebra: all
  geometry: all
  statistics: all
  proportions: all
  numbers:
  - primes
  - sequences
  - money
  - general
  - indices
  #- fdp ## FDP is included
  - calculator
  - time
  - hcflcm
  - rounding
  - settheory
  - estimate
  - standardform
  - factors
  - proof
  - surds
```

Take this scenario. I have elected to not allow Tryal.AI to repeat any question structure more than once (a rule designed to ensure that a GCSE mock paper has a diverse set of questions), but simultaneously told it that it can only use questions from `fdp` (Fractions, Decimals and Percentages). The odds are that Tryal.AI cannot produce 50 substantially different questions on just FDP, particularly as Tryal.AI takes into account the underlying methodology in the question when deciding if a question is the same (and thus **should not** be repeated).

Now you might be saying, "Well ignore the repeat rule and just do it". We could allow Tryal.AI to override rules it has been provided, and generate approximate material. However, this might be seen as disingenuous when the generator has been given an instruction and decided not to follow it. As we bill for material generated, we also felt not providing the material because we cannot do what you've asked is preferable, as it doesn't incur a cost to you.

We're working on ways to better explain what went wrong in generation, but due to the nature of artificial intelligence this isn't always necessarily possible. In fact, there's an entire area of research in the field of AI dedicated to understanding how to [explain when and why an AI failed (or succeeded)](https://en.wikipedia.org/wiki/Explainable_artificial_intelligence)

This does mean however, that the burden of sensible error handling is passed on to Tryal.AI Develop users. In general we have a small family of errors which indicate that the generator has failed, detailed below. 

<aside class="notice">
  In general, if the same call fails more than 2 or 3 times, it's probably not going to succeed. Our generator makes approximately 12 tries before returning a failed result, meaning 3 failed calls, is over 36 failed generation attempts.
</aside>

## Generation Errors
- **500**
  - *GENERATOR_CALL_FAILED* - We couldn't reach the generator and get it to process your request. This typically only happens during maintenance. Maintenance typically doesn't last long, but in exceptionally rare cases, it may result in a request being dropped.
  We recommend retrying this call after waiting a few seconds. If this response persists, you should contact support.
  - *GENERATOR_FAILED* - Indicates that generation has failed, this response can happen for both reasons mentioned above, the rare cases where it is possible to generate material, but in this specific instance Tryal.AI has failed. The more likely case of poor configuration.
- **404**
  - *RETRIEVE_FAILED* - This is typically a data storage issue and not indicative of a configuration issue. It may be that the item your trying to retrieve is no longer being stored by our data storage. Please see storage and retrieval time limits for Tryal.AI material.

