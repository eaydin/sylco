sylco.py
===========

http://about.me/emre.aydin <br>
http://github.com/eaydin/sylco <br>

## About :
This is my first attempt to count the syllables of a word in the English Language, without using the NLTK for Python.

I've discussed the details in my blog post [here](http://eayd.in/?p=232).
Below is the copy of that discussion.</p>

Dependency : Python 2.6.x

Even though it seems like an easy task, counting syllables is very hard in English. After hours of googling I’ve realized that the non-corpus-based algorithms are not perfect, and it’s impossible for them to be. So I wanted to make a better one combining some I’ve read and overcoming the errors I’ve encountered. The goal is to create a step-by-step algorithm using the least amount of dictionary help. Here’s the first time I’m sharing it publicly hoping that it will help someone out.

Even though this is Python based, the important thing is the algorithm. I know it’s not the best way to handle it, but the outcome’s not so bad for a first attempt.

Here are some discussions or algorithms I’ve found on the web.

+ http://allenporter.tumblr.com/post/9776954743/syllables  simple algorithm
+ http://www.howmanysyllables.com/howtocountsyllables.html  some pseudo-algorithm
+ http://www.modulus.com.au/blog/?p=8  a little bit more algorithm
+ http://www.onebloke.com/2011/06/counting-syllables-accurately-in-python-on-google-app-engine/ this uses the nltk library, which is not what I’m looking for, since that’s not the challenge. (yet, it may be the most clever approach for you)

Reading all these and experimenting, I’ve developed my own set of rules, here they go.

## The Algorithm

1. If number of letters <= 3 : return 1
2. If doesn’t end with “ted” or “tes” or “ses” or “ied” or “ies”, discard “es” and “ed” at the end. If it has only 1 vowel or 1 set of consecutive vowels, discard. (like “speed”, “fled” etc.)
3. Discard trailing “e”, except where ending is “le” and isn’t in the le_except array
4. Check if consecutive vowels exists, triplets or pairs, count them as one.
5. Count remaining vowels in the word.
6. Add one if begins with “mc”
7. Add one if ends with “y” but is not surrouned by vowel. (ex. “mickey”)
8. Add one if “y” is surrounded by non-vowels and is not in the last word. (ex. “python”)
9. If begins with “tri-” or “bi-” and is followed by a vowel, add one. (so that “ia” at “triangle” won’t be mistreated by step 4)
10. If ends with “-ian”, should be counted as two syllables, except for “-tian” and “-cian”. (ex. “indian” and “politician” should be handled differently and shouldn’t be mistreated by step 4)
11. If begins with “co-” and is followed by a vowel, check if it exists in the double syllable dictionary, if not, check if in single dictionary and act accordingly. (co_one and co_two dictionaries handle it. Ex. “coach” and “coapt” shouldn’t be treated equally by step 4)
12. If starts with “pre-” and is followed by a vowel, check if exists in the double syllable dictionary, if not, check if in single dictionary and act accordingly. (similar to step 11, but very weak dictionary for the moment)
13. Check for “-n’t” and cross match with dictionary to add syllable. (ex. “doesn’t”, “couldn’t”)
14. Handling the exceptional words. (ex. “serious”, “fortunately”)

## Known bugs

Like I said earlier, this isn’t perfect, so there are some steps to add or modify, but it works just “fine”. Some exceptions should be added such as “evacuate”, “ambulances”, “shuttled”, “anyone” etc… Also it can’t handle some compund words like “facebook”. Counting only “face” would result correctly “1″, and “book” would also come out correct, but due to the “e” letter not being detected as a “silent e”, “facebook” will return “3 syllables.”

## Additional Stuff

The "getFlesch" function also calculates the Flesch Index for an article, might be useful if you're testing something like that.