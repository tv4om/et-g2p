Changelist:
===========

* src/G2P.java -- pseudo-json output
* README.md -- updated examples

Grapheme to phoneme rules for Estonian
======================================

Implements a naive rule-based G2P converter for Estonian. 

Note:
* Compound words should have their compound constituents separated with and underscore in input (e.g. 'hamba_kivi')
 


Compile
-------

	ant 
	
Test
----

	ant test
	
Run
---
	
    $echo "kesk_pank" | ./run.sh
    {"token":"kesk_pank","pronunciations": ["k e s k p a n kk"]}
	$

The error message goes to stdout.

    $ echo -e "palgi_koorem\npaµk\nOECD" | ./run.sh
    {"token":"palgi_koorem","pronunciations": ["p a l k i k o o r e m"]}
    {"token":"paµk","warning":"cannot convert word, reason:Unknown character [µ] in input"}
    {"token":"OECD","pronunciations": ["o e k t","o o e e t s e e t e e"]}
	$

The script reads words from stdin and writes words with their pronunciations back to stdout. Usually it's used to process 
a long list of words at a time:

    $ ./run.sh < example/sample.vocab
    {"token":"kana","pronunciations": ["k a n a"]}
    {"token":"park","pronunciations": ["p a r kk"]}
    {"token":"näinud","pronunciations": ["n ae i n u t","n ae i n t"]}
    {"token":"kontsa","pronunciations": ["k o n t s a"]}
    {"token":"kesk_pank","pronunciations": ["k e s k p a n kk"]}
    {"token":"OECD","pronunciations": ["o e k t","o o e e t s e e t e e"]}
    {"token":"ETV24","pronunciations": ["e e t e e v e e k a k s k ue m m e n t n e l i","e e t e e v e e k a h e k ue m n e n e l j a"]}
    {"token":"ETV24-le","pronunciations": ["e e t e e v e e k a h e k ue m n e n e l j a l e"]}
    {"token":"NATO","pronunciations": ["n a tt o"]}
    {"token":"ABC-pood","pronunciations": ["a p k p o o t","a a p e e t s e e p o o t"]}
    {"token":"René","pronunciations": ["r e n e"]}
    {"token":"Poincaré","pronunciations": ["p o i n kk a r e"]}
	$
	
The script can be also executed from any other directory:

	$ ~/tools/et-g2p/run.sh < vocab.txt


The tool supports user-defined transliteration dictionaries. This allows to define pronunciations for 
words that are pronounced differently from the Estonian rules and which are not defined in the 
built-in exception list.

Each line in the dictionary contains the original word and its transliteration (i.e., its
probable ortographic form as if it was an Estonian word). Multiple transliterations can be given,
seperated by commas. For example:

    Jules      žül
    Henri      henri, anrii
    Poincaré   puangarree


Use the -dict option to set the user dictionary:

    echo "Henri" | ./run.sh -dict tmp.dict
    Henri	a n r i i
    Henri(2)	h e n r i



Bugs
----

Pronunciation of foreign names is based on a short list of exceptions. As a result, for most
English and French names, pronunciation is generated according to Estonian rules, which is 
of course wrong (see `Poincaré` above).


Use as a library
----------------

See `src/java_test` for samples.	 
