# Skills for Speedup Building Android Modules



## 1. Speedup Android kernel building

### 1.1 Get and make use of seperate commands
By referring build/core/Makefile, It shows that, we can see the sequence of commands running while building by passing SHOW_COMMANDS=1 while building Android as below,

```{r, engine='bash', count_lines}
SHOW_COMMANDS=1 V=1 make bootimage -j1 -n >bootimage.txt 
```
From this we can extract the commands which are necessary for our case, and we can put into a script to build. eg, bootimage.sh

### 1.2 Specify the Android.mk file
For building just the kernel this saved me a ton of time in Android L,M,N:-
```{r, engine='bash', count_lines}
m -j8 ONE_SHOT_MAKEFILE=build/target/board/Android.mk bootimage
```
See details in build/envsetup.sh:
``` bash
function m()
{
    local T=$(gettop)
    local DRV=$(getdriver $T)
    if [ "$T" ]; then
        $DRV make -C $T -f build/core/main.mk $@
    else
        echo "Couldn't locate the top of the tree.  Try setting TOP."
        return 1
    fi
}

function mmm()
{
        ONE_SHOT_MAKEFILE="$MAKEFILE" $DRV make -C $T -f build/core/main.mk $DASH_ARGS $MODULES $ARGS
}
```
build/core/main.mk#516:
``` Makefile
ifneq ($(ONE_SHOT_MAKEFILE),)
# We've probably been invoked by the "mm" shell function
# with a subdirectory's makefile.
include $(ONE_SHOT_MAKEFILE)
```

### 1.3 Normal ways to build kernel
```bash
m -j8 SHOW_COMMANDS=1 V=1 ONE_SHOT_MAKEFILE=build/target/board/Android.mk bootimage 2>&1 | tee build.log
```
http://opensourceforu.com/2014/09/building-the-android-platform-compile-the-kernel/
