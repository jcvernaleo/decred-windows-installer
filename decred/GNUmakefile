# only 3 digits are used, and must increment for upgrades to work
VERS=0.5.0
VERNAME=$(VERS)-beta
INSTALLERNAME="Decred Installer"
MANIFEST=manifest-paymetheus-v

all: msi msi64 sha cleanobjs

current: msi msi64 sha cleanobjs

TOOLSDIR=../tools

# Wix executables
WIXDIR=$(WIX)bin
HEAT="$(WIXDIR)/heat.exe"
CANDLE="$(WIXDIR)/candle.exe"
LIT="$(WIXDIR)/lit.exe"
LIGHT="$(WIXDIR)/light.exe"

update:

stage:

sha:
	sha256sum decred_$(VERNAME)_x86.msi > $(MANIFEST)$(VERS).txt
	sha256sum decred_$(VERNAME)_x64.msi >> $(MANIFEST)$(VERS).txt

msi: decred.wxs stage
	$(CANDLE) -nologo -arch x86 -dVersion="$(VERS)" -o decred_x86.wixobj \
	    decred.wxs
	$(LIGHT) -nologo -o decred_$(VERNAME)_x86.msi decred_x86.wixobj \
	     ../dcrticketbuyer/*_x86.wixlib ../paymetheus/*_x86.wixlib \
	     -ext WixUIExtension -ext WixUtilExtension -b . -b ../resource \
	     -b ../dcrticketbuyer -b ../paymetheus
	sh "$(TOOLSDIR)/sign.sh" $(INSTALLERNAME) "$(CURDIR)/*.msi"

msi64: decred.wxs stage
	$(CANDLE) -nologo -arch x64 -dVersion="$(VERS)" -o decred_x64.wixobj \
	    decred.wxs
	$(LIGHT) -nologo -o decred_$(VERNAME)_x64.msi decred_x64.wixobj \
	     ../dcrticketbuyer/*_x64.wixlib ../paymetheus/*_x64.wixlib \
	     -ext WixUIExtension -ext WixUtilExtension -b . -b ../resource \
	     -b ../dcrticketbuyer -b ../paymetheus
	sh "$(TOOLSDIR)/sign.sh" $(INSTALLERNAME) "$(CURDIR)/*.msi"

cleanobjs:
	rm -f decred*.wixobj decred*.wixpdb dcr*.wixlib

clean: cleanobjs
	rm -f decred*.msi
	rm -f $(MANIFEST)*.txt

.PHONY: all current msi msi64
