- The internal state of a Mersenne Twister contains 624 integral values. For `std::mt19937`, these values have type `std::uint_fast32_t`, which could be 32-bit or 64-bit in size. For `std::mt19937_64`, these values have type `std::uint_fast64_t`, which are typically 64-bits each.
- In the examples above, where we seed from the clock or std::random_device, our seed is only a single value. This means we’re essentially initializing 624 values using a single value, which is significantly underseeding the Mersenne Twister PRNG. The Random library does the best it can to fill in the remaining 623 values with “random” data… but it can’t work magic. Underseeded PRNG can generate results that are suboptimal for applications that need the highest quality results. For example, seeding `std::mt19937` with a single 32-bit value will never generate the number `42` as its first output.
---
- First, let’s talk about `std::seed_seq` (which stands for “seed sequence”). In the prior lesson, we mentioned that ideally we want our seed data to be as many bits as the state of our PRNG, or our PRNG will be underseeded. But in many cases (especially when our PRNG has a large state), we won’t have that many bits of randomized seed data.
- `std::seed_seq` is a type that was designed to help with this. We can pass it as many randomized values as we have, and then it will generate as many additional unbiased seed values as needed to initialize a PRNG’s state. If you initialize `std::seed_seq` with a single value (e.g. from `std::random_device`) and then initialize a Mersenne Twister with the `std::seed_seq` object, `std::seed_seq` will generate 623 values of additional seed data. That’s not going to be any better than just directly using a single value. But the real power of `std::seed_seq` is that we can give it more than one piece of data, and the more pieces of random data we can give `std::seed_seq` to work with, the better. So the easiest idea is to simply use `std::random_device` to give `std::seed_seq` more data to work with. If we initialize `std::seed_seq` with 8 values from `std::random_device` instead of 1, then the remaining values generated by `std::seed_seq` should be much better:

```cpp
#include <iostream>
#include <random>

int main()
{
	std::random_device rd{};
	std::seed_seq ss{ rd(), rd(), rd(), rd(), rd(), rd(), rd(), rd() }; // get 8 integers of random numbers from std::random_device for our seed
	std::mt19937 mt{ ss }; // initialize our Mersenne Twister with the std::seed_seq

	// Create a reusable random number generator that generates uniform numbers between 1 and 6
	std::uniform_int_distribution die6{ 1, 6 }; // for C++14, use std::uniform_int_distribution<> die6{ 1, 6 };

	// Print a bunch of random numbers
	for (int count{ 1 }; count <= 40; ++count)
	{
		std::cout << die6(mt) << '\t'; // generate a roll of the die here

		// If we've printed 10 numbers, start a new row
		if (count % 10 == 0)
			std::cout << '\n';
	}

	return 0;
}
```

- This is pretty straightforward so there isn’t much reason not to do this at a minimum. The results from seeding a `std::mt_19937` with 8 seed values instead of a single value should be much better.
 ```ad-question
 Why not give std::seed_seq 624 values from `std::random_device`?
```

```ad-Answer
You can, but this is likely to be slow, and risks depleting the pool of random numbers that `std::random_device` uses.
```
- You can also use other “random” inputs to `std::seed_seq`. We’ve already shown you how to get a value from the clock, so you can throw that in easily. Other things that are sometimes used include the current thread id, the address of particular functions, the user’s id, the process id, etc… Doing that is beyond the scope of this article, but [this article](https://www.pcg-random.org/posts/ease-of-use-without-loss-of-power.html) has some context and a link to [randutils.hpp](https://gist.github.com/imneme/540829265469e673d045) that implements this.

- An alternate path is to use a different PRNG with a smaller state. Many good PRNGs use 64 or 128 bits of state, which can easily be initialized using `std::seed_seq` filled with 8 calls to `std::random_device`.