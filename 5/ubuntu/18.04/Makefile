CHICKEN_VERSION=5.0.0
TAG=$(CHICKEN_VERSION)-ubuntu18.04

build:
	docker build -t chicken:$(TAG) .
build-ver:
	docker build -t chicken:$(TAG) --build-arg PREFIX=/usr/local/chicken-$(CHICKEN_VERSION) .
	if docker volume inspect chicken >/dev/null 2>&1; then docker volume rm chicken; fi
	docker run -it --rm -v chicken:/usr/local/chicken-$(CHICKEN_VERSION) chicken:$(TAG) true
	docker run -it --rm -v chicken:/src/usr/local/chicken-$(CHICKEN_VERSION) -v `pwd`/deb:/deb cdrx/fpm-ubuntu:18.04 \
		fpm -s dir -t deb -p /deb \
		  -n chicken-$(CHICKEN_VERSION) \
		  -v 1 \
		  .
	# additions: changelog needs to be set or zeroed -- default path is /usr/share/doc
	# does deb pkgs allow multi-install
	docker volume rm chicken
run:
	docker run -it --rm chicken:$(TAG)

