SHELL:=/usr/bin/env bash

VERSION=0.0.1
JV_URL="https://clock.dzdz.cz"
define BUILD_FLAGS
-n "Clock" \
--disable-dev-tools \
--min-width 750 \
--min-height 510 \
--width 750 \
--height 510 \
--app-version ${VERSION}
endef

default: help
# via https://marmelab.com/blog/2016/02/29/auto-documented-makefile.html
.PHONY: help
help:
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

.PHONY: check-deps
check-deps:  ## Verify build dependencies are installed
	@command -v nativefier >/dev/null 2>&1 || echo "[!] Missing nativefier: npm install -g nativefier"

.PHONY: clean-mac
clean-mac:  ## Clean mac build directory
	rm -rf ./mac

.PHONY: build-mac
build-mac: clean-mac check-deps  ## Build app for macOS/x64
	mkdir ./mac
	nativefier ${JV_URL} ${BUILD_FLAGS} \
		-i ../icon/clock-flat/512x512.png \
		--fast-quit \
		--darwin-dark-mode-support \
		-p mac -a x64 ./mac

.PHONY: install-mac
install-mac: build-mac  ## Build & install to /Applications on macOS
	cp -R "./mac/Clock-darwin-x64/Clock.app" /Applications
