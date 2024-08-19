
## UNFREE 

used House of free to exploit:

since no free, a free chunk needs to be created using ` _int_free` in sysmalloc, then use unsorted bin attack to overwrite `io_list_all` in libc  to gain control flow


sysmalloc can be triggered by filling top chunk so that it can be called to either allocate new memory area by either extending the heap or calling mmap to get new memory area. There are specific conditions to trigger `_int_free` in sysmalloc:
- larger than MINSIZE(0x10)
- smaller than `need size + MINSIZE`
- prev inuse is set
- `old_top + oldsize` must be aligned a page.

whenever glibc detects memory corruptions it goes with abort routines, in which all streams are flushed and enter the `_IO_flush_all_lockp` and use the `_IO_FILE` object, which is `_IO_list_all` in it. `_IO_FILE` uses virtual function table `_IO_JUMP_t` to do stuff, which is forged.



---
another challenge in litcitf could be solved using FSOP(file stream oriented programming)
so need to focus more on FSOP techniques.


---

[Deep dive into FSOP · Niftic's blog](https://niftic.ca/posts/fsop/#introduction)

[House of Orange - Nightmare (guyinatuxedo.github.io)](https://guyinatuxedo.github.io/43-house_of_orange/house_orange_exp/index.html)

[Pwning My Life: HITCON CTF Qual 2016 - House of Orange Write up (4ngelboy.blogspot.com)](https://4ngelboy.blogspot.com/2016/10/hitcon-ctf-qual-2016-house-of-orange.html)




