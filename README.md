# Fuzzing Guide for CALC
---

### 1. How to build from source

Build with default compiler and flags with:
```shell
make
```

If you want to fuzz, please compile with afl instrumentation like using `afl-gcc-fast` etc.

Generally, we can `make` it with specific `CC` environment variable. However, project `calc`'s `Makefile` doesn't read this environment variable. So we need to do some change in `Makefile`.

```gitdiff
diff --git a/Makefile b/Makefile
index b12766d..7fdd970 100644
--- a/Makefile
+++ b/Makefile
@@ -91,6 +91,9 @@ include ${TARGET_MKF}
 #
 # S= >/dev/null 2>&1   silence ${CC} output during hsrc file formation
 # S=                   full ${CC} output during hsrc file formation
+CC = ${HOME}/AFLPlusPlus/afl-gcc-fast -g -ggdb
 #
 # E= 2>/dev/null       silence command stderr during hsrc file formation
 # E=                   full command stderr during hsrc file formation
```

After patch, you can re`make` it.

