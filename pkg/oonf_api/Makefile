PKG_NAME=oonf_api
PKG_URL=http://olsr.org/git/oonf.git
PKG_VERSION=v0.3.0
PKG_DIR=$(CURDIR)/$(PKG_NAME)

ifneq ($(RIOTBOARD),)
include $(RIOTBOARD)/$(BOARD)/Makefile.include
endif

ifneq ($(RIOTBASE),)
INCLUDES += -I$(RIOTBASE)/sys/include -I$(RIOTBASE)/sys/net/include \
			-I$(RIOTBASE)/sys/posix/include
endif

MODULE:=$(shell basename $(CURDIR))

.PHONY: all clean patch reset

all: patch
	"$(MAKE)" -C $(PKG_DIR)
	"$(MAKE)" $(BINDIR)$(MODULE).a

patch: $(PKG_DIR)/Makefile

$(PKG_DIR)/Makefile: $(PKG_DIR)/.git/config
	cd "$(PKG_DIR)" && git am --ignore-whitespace "$(CURDIR)"/*.patch

$(PKG_DIR)/.git/config:
	test -d "$(PKG_DIR)" || git clone "$(PKG_URL)" "$(PKG_DIR)"; \
		cd "$(PKG_DIR)" && git checkout -f "$(PKG_VERSION)"

clean::
	@echo "Cleaning up oonf_api package..."
	@cd "$(PKG_DIR)" 2> /dev/null > /dev/null && \
		git clean -x -f && \
		git am --abort && \
		git reset --hard "$(PKG_VERSION)" && \
		$(MAKE) patch || true

distclean::
	rm -rf $(PKG_DIR)

$(BINDIR)$(MODULE).a: $(BINDIR)oonf_*.a
	mkdir -p $(BINDIR)$(MODULE); cd $(BINDIR)$(MODULE); for var in $?; do ar -x $$var; done; ar -r -c -s $(BINDIR)$(MODULE).a *.o

Makefile.include:
	@true
