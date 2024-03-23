if WANT_JANSSON
JANSSON_INCLUDES= -I$(top_srcdir)/compat/jansson
else
JANSSON_INCLUDES=
endif

EXTRA_DIST  = example-cfg.json nomacro.pl

SUBDIRS     = compat

ALL_INCLUDES    = @PTHREAD_FLAGS@ -fno-strict-aliasing $(JANSSON_INCLUDES) -I.

bin_PROGRAMS    = cpuminer

dist_man_MANS   = cpuminer.1

cpuminer_SOURCES = \
  cpu-miner.c \
  algo/x16/minotaur.c \
  algo/yespower/yespower-gate.c \
  algo/yespower/yespower-blake2b.c \
  algo/yespower/crypto/hmac-blake2b.c \
  algo/yespower/yescrypt-r8g.c \
  algo/yespower/yespower-opt.c \
  algo/yespower/yespower-ref.c \
  algo/yespower/yespower-blake2b-ref.c

disable_flags =

if USE_ASM
   cpuminer_SOURCES += asm/neoscrypt_asm.S
else
   disable_flags += -DNOASM
endif

if HAVE_WINDOWS
   cpuminer_SOURCES += compat/winansi.c
endif

cpuminer_LDFLAGS    = @LDFLAGS@
cpuminer_LDADD  = @LIBCURL@ @JANSSON_LIBS@ @PTHREAD_LIBS@ @WS2_LIBS@  -lgmp
cpuminer_CPPFLAGS = @LIBCURL_CPPFLAGS@ $(ALL_INCLUDES)
cpuminer_CFLAGS   = -Wno-pointer-sign -Wno-pointer-to-int-cast $(disable_flags)

if HAVE_WINDOWS
cpuminer_CFLAGS += -Wl,--stack,10485760
endif

if HAVE_WINDOWS
# use to profile an object
# gprof_cflags = -pg -g3
# cpuminer_LDFLAGS += -pg
# cpuminer_CFLAGS += -fno-inline-functions -static

# copy/paste from generated Makefile
common_ccflags = $(DEFS) $(ALL_INCLUDES) $(cpuminer_CPPFLAGS) $(CPPFLAGS) $(cpuminer_CFLAGS) $(CFLAGS)

# special CFLAGS (if you find a simpler way to do that tell me ;)
cpuminer-neoscrypt.o: neoscrypt.c
    @echo "CUSTOM ${@}: ${filter %.o,${^}} ${filter %.c,${^}}"
    $(CC) $(common_ccflags) -g -O3 $(gprof_cflags) -MT $@ -MD -MP -c -o $@ $<

endif
