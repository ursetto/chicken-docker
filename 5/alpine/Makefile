CHICKEN_VERSION=5.0.0
TAG=$(CHICKEN_VERSION)-alpine

build:
	docker build -t chicken:$(TAG) .
run:
	docker run -it --rm chicken:$(TAG)
