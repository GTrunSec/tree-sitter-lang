CC?=gcc
CFLAGS+=-std=c99 -Iexternals/tree-sitter-go/src/ -U_FORTIFY_SOURCE -D_FORTIFY_SOURCE=1 -O2 -I../../externals/tree-sitter/lib/include \
    -I../../includes/
CXXFLAGS+=-Iexternals/tree-sitter-go/src/ -U_FORTIFY_SOURCE -D_FORTIFY_SOURCE=1 -O2 -I../../externals/tree-sitter/lib/include \
    -I../../includes/
LDFLAGS+=

# Depend on the tree-sitter parser C file and the Emacs module source
sources_c=externals/tree-sitter-go/src/parser.c $(wildcard *.c)

include version.mk

all: dist

version.mk: tree-sitter-lang-go-pkg.el
	@sed -n 's/(define-package ".*" "\([0-9\.]*\)"/VERSION=\1/p' $< > version.mk

tree-sitter-lang-go-module.so: $(sources_c:.c=.o) $(sources_cc:.cc=.o)
	$(CC) -shared -fPIC $(LDFLAGS) -o $@ $^

%.o: %.cc
	$(CC) -fPIC -c $(CXXFLAGS) -o $@ $<

%.o: %.c
	$(CC) -fPIC -c $(CFLAGS) -o $@ $<

dist: tree-sitter-lang-go-$(VERSION).tar

tree-sitter-lang-go-%.tar: tree-sitter-lang-go-module.so $(wildcard *.el)
	mkdir "tree-sitter-lang-go-$*"
	cp $^ "tree-sitter-lang-go-$*"
	tar -cf $@ "tree-sitter-lang-go-$*"
	rm -r "tree-sitter-lang-go-$*"

clean:
	rm -f $(sources_c:.c=.o) $(sources_cc:.cc=.o) tree-sitter-lang-go-module.so

submod:
	git submodule update --init externals/tree-sitter-go

.PHONY: clean submod dist
