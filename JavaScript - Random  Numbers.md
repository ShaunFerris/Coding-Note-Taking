#javascript #math #random #numbers #basics

The builtin JS function `Math.random()` returns a random decimal number between 0 and 1 exclusive (ie can be zero but cannot be one). This function is extended to produce other kinds of random number in JS, rather than using a range adjusted function like pythons `random.randint()`.

To generate random whole numbers in JS you would typically use and expression like:
`Math.floor(Math.random() * 20);`.
This expression will generate random integers with a min of zero and a max of 19. Th outer function here is floor division to make sure that only whole number integers are returned.

To generate whole numbers within a predefined range, use this expression:
`Math.floor(Math.random() * (max - min)) + min`
