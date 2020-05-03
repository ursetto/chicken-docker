CHICKEN_VERSION=5.0.0
ITER := 1
TAG=$(CHICKEN_VERSION)-ubuntu18.04

build:
	docker build -t chicken:$(TAG) .
build-ver:
	docker build -t chicken:$(TAG) --build-arg PREFIX=/usr/local/chicken-$(CHICKEN_VERSION) .
	@# Create a temp volume to share between the chicken container and the fpm container.
	@#  [This syntax sets the VOL makefile var instead of the VOL shell var (which requires
	@#   a bunch of continuation lines and comments stripped, or .ONESHELL which is not supported by macOS).]
	$(eval VOL=$(shell docker volume create))
	@# Run the container once to copy the Chicken files into empty 'chicken' volume.
	docker run -it --rm -v $(VOL):/usr/local/chicken-$(CHICKEN_VERSION) chicken:$(TAG) true
	@# Run fpm image to package the files in the chicken volume, mounting it into /src (the image's working dir).
	@# The output package is written to /deb (host dir ./deb).
	docker run -it --rm -v $(VOL):/src/usr/local/chicken-$(CHICKEN_VERSION) -v `pwd`/deb:/deb \
	  cdrx/fpm-ubuntu:18.04 \
		fpm -f -s dir -t deb -p /deb \
		  -n chicken-$(CHICKEN_VERSION) \
		  -v $(CHICKEN_VERSION) \
		  --iteration $(ITER) \
		  --license BSD \
		  --url http://www.call-cc.org \
		  --maintainer jim@3e8.org \
		  --vendor jim@3e8.org \
		  --description 'CHICKEN Scheme compiler, runtime and development environment' \
		  .
	@# Chicken version is included in package name to allow multiple concurrent installs.
	@# 
	@# additions:
	@# maintainer, homepage, license, vendor, description, changelog
	@# dependencies!
	
	@# Clean up our temporary docker volume
	docker volume rm $(VOL) > /dev/null
run:
	docker run -it --rm chicken:$(TAG)

