# House of Einherjar - Yet Another Heap Exploitation Technique on GLIBC
I spoke about my heap exploitation technique, House of Einherjar, [at CODE BLUE as one of U-25 speakers in 2016](http://codeblue.jp/2016/en/contents/speakers.html#speaker-matsukuma) and here are some stuffs used in the talking.   

## TL;DR
Glibc malloc's chunk uses Boundary Tag Method and Linked List for dynamic memory management and the former has a weakness which may lead to write-what-where primitive.   
House of Einherjar forces malloc() to return nearly-arbitrary value by abusing `struct malloc_chunks`'s prevsize member. This `prevsize` can be shared with the chunk which is well-sized (roughly speaking, its size is `SIZE_SZ * 2n`) thus its area can be written by normal write. If NUL byte(`'\0'`) poisoning occurs on this area and `PREV_INUSE` bit is overwritten, the previous contiguous chunk will be treated as an already free()'ed chunk. 
This corruption leads an illegal moving chunk to a nearly-arbitrary address due to consolidating chunks in `_int_free()` and the linking a chunk to unsorted bin. If a well crafted data on the target address, pwner can get the address.   

## Stuffs
- [Slide (en)](https://raw.githubusercontent.com/st4g3r/House-of-Einherjar-CB2016/master/slide.pdf)
- [PoC](https://gist.github.com/st4g3r/939fb0753c57c79fc56a50ea1e9e5312) 

## Any Question
[Reply or DM to @st4g3r](https://twitter.com/st4g3r)
