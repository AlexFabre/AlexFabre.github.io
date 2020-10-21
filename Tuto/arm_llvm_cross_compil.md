# ARM-cortex LLVM cross compilation

## Install dependencies

If you see this librarie missing : "stubs-32.h"

```sh
sudo apt install libc6-dev-i386
```

## Download LLVM sources


## Refactor sources

```sh
#!/bin/bash
 
echo "Setting CC variable to arm-none-eabi-gcc..."
sed -i 's/RM := .*/&\nCC := arm-none-eabi-gcc/g' Debug/makefile
 
 
echo "Replacing compiler with CC variable..."
for mkfile in $(grep -rl --exclude=makefile arm-none-eabi-gcc Debug)
do
    sed -i 's/arm-none-eabi-gcc/$(CC)/g' $mkfile
done
 
 
echo "replacing -Og with one clang recognizes..."
for mkfile in $(grep -rl '\-Og' Debug)
do
    sed -i 's/-Og/-O2/g' $mkfile
done
 
 
FIND='\([ \t]*\)\(.*:[ ]"r"[ ](\)\([a-zA-Z0-9_]*\)\()[ ]:[ ]"vfpcc");\)'
REPLACE='#ifdef __clang__\n\1__builtin_arm_set_fpscr(\3);\n#else\n&\n#endif'
 
echo "replacing vfpcc with clang builtin"
for srcfile in $(grep -rl vfpcc .)
do
if [[ $srcfile != *$0 ]]
then
    echo "add clang builtin for vfpcc $srcfile"
    sed -i 's/'"$FIND"'/'"$REPLACE"'/g' $srcfile
fi
done
 
echo -e "you can use scan-build for analysis.\n\n"
```
or compile with option -mfpu=none

scan-build --use-cc=/usr/bin/arm-none-eabi-gcc --analyzer-target=arm-none-eabi make -C Debug