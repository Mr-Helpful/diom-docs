Due to the fact that:
1. Lexing is one of the steps of compilation that can be performed very quickly
2. Lexed tokens can often be stored more efficiently than raw text[^1]
3. Lexing <= the number of characters fed into it

The first stage of the compiler that I'm going to save for incremental compilation is the Tokens representing the raw text, not the text itself.

This is especially important as the first step of incremental compilation used in Diom is a simple diff between the inputs (see edit distance: [my own explanation](https://github.com/Mr-Helpful/preventable-deaths-scraper/tree/main/src/correct), [wikipedia](https://en.wikipedia.org/wiki/Levenshtein_distance)), which typically has complexity $O(mn)$ in the lengths of their two inputs. Hence, minimising the length of both inputs gives us an $O(n^2)$ advantage.

[^1]: this is mostly due to the fact that tokens, by their nature, contain a subset of possible text, rather than the full raw text.