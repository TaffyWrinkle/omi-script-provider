TOP?=$(shell cd ../..; pwd)
include $(TOP)/scriptprovider/config.mak

STAGING_DIR:=$(OSP_OUTPUTDIR)/staging
INTERMEDIATE_DIR:=$(OSP_OUTPUTDIR)/intermediate
RELEASE_DIR:=$(OSP_OUTPUTDIR)/release

IB_DIR:=$(TOP)/pal/installer/InstallBuilder

UNAME_M:=$(shell uname -m)
ifeq ($(UNAME_M),x86_64)
 PF_ARCH := x64
else
 PF_ARCH := x86
endif

SHORT_NAME=$(CONFIG_PRODUCT)
LONG_NAME=$(CONFIG_FULLPRODUCT)
PF_MAJOR=$(CONFIG_MAJOR)
PF_MINOR=$(CONFIG_MINOR)
OMI_SCRIPTPROVIDER_VERSION_BUILD=$(CONFIG_OMI_SCRIPTPROVIDER_VERSION_BUILD)
PYTHON_VERSION=$(CONFIG_PYTHON_VERSION)
PROVIDES=omiscriptproviderpkg
DESCRIPTION=Enables OMI providers to be written in scripting languages for Linux
RUN_AS_USER=root
PACKAGE_NAME=$(SHORT_NAME)-$(CONFIG_VERSION)-$(OMI_SCRIPTPROVIDER_VERSION_BUILD).Python-$(PYTHON_VERSION).$(PF_ARCH)

ifeq ("$(wildcard /usr/bin/dpkg-deb)","")
DPKG_LOCATION=--DPKG_LOCATION="$(IB_DIR)/tools/bin/dpkg-deb-$(PF_ARCH)"
else
DPKG_LOCATION=
endif


all:
	@echo "========================= Make OMI Script Provider installer RPM"
	sudo rm -rf $(STAGING_DIR) $(INTERMEDIATE_DIR) $(RELEASE_DIR)/*.rpm \
		$(RELEASE_DIR)/*.rpm
	mkdir -p $(RELEASE_DIR) $(STAGING_DIR) $(INTERMEDIATE_DIR)
	python $(IB_DIR)/installbuilder.py \
		--BASE_DIR=$(TOP)/scriptprovider \
		--TARGET_DIR=$(RELEASE_DIR) \
		--INTERMEDIATE_DIR=$(INTERMEDIATE_DIR) \
		--STAGING_DIR=$(STAGING_DIR) \
		--PACKAGE_TYPE=RPM \
		--PF=Linux \
		--PFMAJOR=$(PF_MAJOR) \
		--PFMINOR=$(PF_MINOR) \
		--PFARCH=$(PF_ARCH) \
		--PFDISTRO=ULINUX \
		--VERSION=$(CONFIG_VERSION) \
		--RELEASE=$(OMI_SCRIPTPROVIDER_VERSION_BUILD) \
		--SHORT_NAME=$(SHORT_NAME) \
		--LONG_NAME="$(LONG_NAME)" \
		--PROVIDES=$(PROVIDES) \
		--DESCRIPTION="$(DESCRIPTION)" \
		--RUN_AS_USER="$(RUN_AS_USER)" \
		--OUTPUTFILE=$(PACKAGE_NAME) \
		--DATAFILE_PATH="$(TOP)/scriptprovider/installbuilder" \
		Base_OMIScriptProvider.data Linux.data Linux_RPM.data

	@echo "========================= Make OMI Script Provider installer DPKG"
	sudo rm -rf $(STAGING_DIR) $(INTERMEDIATE_DIR) 
	mkdir -p $(RELEASE_DIR) $(STAGING_DIR) $(INTERMEDIATE_DIR)
	python $(IB_DIR)/installbuilder.py \
		--BASE_DIR=$(TOP)/scriptprovider \
		--TARGET_DIR=$(RELEASE_DIR) \
		--INTERMEDIATE_DIR=$(INTERMEDIATE_DIR) \
		--STAGING_DIR=$(STAGING_DIR) \
		--PACKAGE_TYPE=DPKG \
		--PF=Linux \
		--PFMAJOR=$(PF_MAJOR) \
		--PFMINOR=$(PF_MINOR) \
		--PFARCH=$(PF_ARCH) \
		--PFDISTRO=ULINUX \
		--VERSION=$(CONFIG_VERSION) \
		--RELEASE=$(OMI_SCRIPTPROVIDER_VERSION_BUILD) \
		--SHORT_NAME=$(SHORT_NAME) \
		--LONG_NAME="$(LONG_NAME)" \
		--PROVIDES=$(PROVIDES) \
		--DESCRIPTION="$(DESCRIPTION)" \
		--RUN_AS_USER="$(RUN_AS_USER)" \
		--OUTPUTFILE=$(PACKAGE_NAME) \
		$(DPKG_LOCATION) \
		--DATAFILE_PATH="$(TOP)/scriptprovider/installbuilder" \
		Base_OMIScriptProvider.data Linux.data Linux_DPKG.data
