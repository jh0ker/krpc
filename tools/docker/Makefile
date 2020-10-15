.PHONY: build test deploy

VERSION=`cat VERSION.txt`

build:
	tar --exclude="bazel-*" --exclude=".git*" --exclude="tools/docker/krpc.tar*" \
	    -cf ./krpc.tar ../..
	tar -C ../../lib/ksp -cvzf ./ksp-`(cd ../..; tools/ksp-version.sh)`.tar.gz KSP_Data/Managed
	docker build \
	  --build-arg ksp_version=`(cd ../..; tools/ksp-version.sh)` \
	  -t krpc/buildenv .
	rm krpc.tar
	rm ./ksp-`(cd ../..; tools/ksp-version.sh)`.tar.gz
	docker tag krpc/buildenv krpc/buildenv:$(VERSION)

test:
	docker run -t -i krpc/buildenv:$(VERSION)

deploy:
	docker push krpc/buildenv
	docker push krpc/buildenv:$(VERSION)
