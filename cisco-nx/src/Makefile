
JAVA_PACKAGE = com.tailf.packages.ned.nexus
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

YANG = $(wildcard yang/*.yang)
SUBMODULES = $(shell grep -l belongs-to yang/*.yang)
YANG_MODULES = $(filter-out $(SUBMODULES),$(YANG))

FXS = $(YANG_MODULES:yang/%.yang=ncsc-out/modules/fxs/%.fxs)

fxs:	$(FXS)


ncsc-out/modules/fxs/tailf-ned-cisco-nx.fxs:\
	yang/tailf-ned-cisco-nx.yang
	$(NCSC) --ncs-compile-module $< \
                --ncs-device-dir ncsc-out \
	  	--ncs-skip-statistics \
                --ncs-device-type cli-ned && \
	  cp $@ ../load-dir
	$(NCSC) $(JFLAGS)/CiscoNexus.java $@

ncsc-out/modules/fxs/tailf-ned-cisco-nx-id.fxs:\
	yang/tailf-ned-cisco-nx-id.yang
	$(NCSC) --ncs-compile-module $< \
                --ncs-device-dir ncsc-out \
	  	--ncs-skip-statistics \
                --ncs-device-type cli-ned && \
	  cp $@ ../load-dir
	$(NCSC) $(JFLAGS)/CiscoNexusId.java $@

javac:
	cd java && ant -q all

clean:
	rm -rf ncsc-out/* ../load-dir/* ncsc-out/*
	rm -f ../jar/*.jar
	rm -f java/src/$(JDIR)/$(NS)/*.java
	cd java && ant clean
	cd ../netsim && $(MAKE) clean

