SHELL:=/usr/bin/env bash

VERSION=0.0.2
URL="https://clock.dzdz.cz"
define BUILD_FLAGS
-n "Clock" \
--disable-dev-tools \
--min-width 750 \
--min-height 510 \
--width 750 \
--height 510 \
--fast-quit \
--darwin-dark-mode-support \
--app-version ${VERSION} \
-i ../icon/clock-flat/512x512.png
endef

default: help
# via https://marmelab.com/blog/2016/02/29/auto-documented-makefile.html
.PHONY: help
help:
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

.PHONY: check-deps
check-deps:  ## Verify build dependencies are installed
	@command -v nativefier >/dev/null 2>&1 || echo "[!] Missing nativefier: npm install -g nativefier"

.PHONY: clean
clean:  ## Clean build output directory
	rm -rf ./out

.PHONY: build
build: clean check-deps  ## Build app for the current platform
	mkdir -p ./out
	npm exec nativefier -- ${URL} ${BUILD_FLAGS} ./out

.PHONY: build-all
build-all: clean check-deps  ## Build and package app for many supported platforms
	mkdir -p ./out
	npm exec nativefier -- ${URL} ${BUILD_FLAGS} -p mac -a x64 ./out
	pushd "./out/Clock-darwin-x64" &&  zip -r ../clock-${VERSION}-macos-x64.zip "./Clock.app" && popd
	npm exec nativefier -- ${URL} ${BUILD_FLAGS} -p mac -a arm64 ./out
	pushd "./out/Clock-darwin-arm64" &&  zip -r ../clock-${VERSION}-macos-arm.zip "./Clock.app" && popd

.PHONY: install-mac
install-mac: build  ## Build & install to /Applications (for macOS, x64 or arm)
	cp -R "./out/Clock-darwin-x64/Clock.app" /Applications || cp -R "./out/Clock-darwin-arm64/Clock.app" /Applications
	rm -rf ./out
