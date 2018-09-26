
JAVA_PACKAGE = com.tailf.packages.ned.a10
JDIR = $(shell echo $(JAVA_PACKAGE) | sed 's/\./\//g')

NS  = namespaces

JFLAGS = --java-disable-prefix \
         --exclude-enums \
         --fail-on-warnings \
         --java-package $(JAVA_PACKAGE).$(NS) \
         --emit-java java/src/$(JDIR)/namespaces

all:	fxs javac
	cd ../netsim && $(MAKE) all

# Include standard NCS examples build definitions and rules
include $(NCS_DIR)/src/ncs/build/include.ncs.mk

SRC = $(wildcard yang/*.yang)
FXS = $(SRC:yang/%.yang=ncsc-out/modules/fxs/%.fxs)

fxs:	$(FXS)

ncsc-out/modules/fxs/%.fxs:	yang/%.yang
	$(NCSC) --ncs-compile-module ./yang/$*.yang \
                --ncs-device-dir ncsc-out \
                --ncs-skip-statistics \
                --ncs-device-type cli-ned && \
	  cp ncsc-out/modules/fxs/* ../load-dir
	$(NCSC) $(JFLAGS)/$*.java $@


javac:
	cd java && ant -q all

clean:
	rm -rf ncsc-out/* ../load-dir/* ncsc-out/*
	rm -f ../jar/*.jar
	rm -f java/src/$(JDIR)/$(NS)/*.java
	cd java && ant clean
	cd ../netsim && $(MAKE) clean
