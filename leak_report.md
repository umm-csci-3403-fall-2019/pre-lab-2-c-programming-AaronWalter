# Leak report

# The source of the leak is in the function strip, where we use calloc to make a pointer to a string. We return that pointer, so we need to close the leak somewhere up the chain, in a function that directly or indirectly calls strip. We do this in is_clean, which receives the pointer when it calls strip. We use the value of cleaned to construct something else, result. is_clean returns result, so at this point we can free up cleaned.

# Additionally, valgrind wasn't happy about an inappropriate use of free, in the case where strip returns "". We fix this error by having strip return stdup("") instead. Praise be to StackOverflow.
