#
# Copyright (c) 1996, 2011, Oracle and/or its affiliates. All rights reserved.
# DO NOT ALTER OR REMOVE COPYRIGHT NOTICES OR THIS FILE HEADER.
#
# This code is free software; you can redistribute it and/or modify it
# under the terms of the GNU General Public License version 2 only, as
# published by the Free Software Foundation.  Oracle designates this
# particular file as subject to the "Classpath" exception as provided
# by Oracle in the LICENSE file that accompanied this code.
#
# This code is distributed in the hope that it will be useful, but WITHOUT
# ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
# FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License
# version 2 for more details (a copy is included in the LICENSE file that
# accompanied this code).
#
# You should have received a copy of the GNU General Public License version
# 2 along with this work; if not, write to the Free Software Foundation,
# Inc., 51 Franklin St, Fifth Floor, Boston, MA 02110-1301 USA.
#
# Please contact Oracle, 500 Oracle Parkway, Redwood Shores, CA 94065 USA
# or visit www.oracle.com if you need additional information or have any
# questions.
#

BUILDDIR = ../..
PACKAGE = java.security
PRODUCT = sun
include $(BUILDDIR)/common/Defs.gmk

#
# Rules
#
AUTO_FILES_JAVA_DIRS = java/security

#
# Directories
#

# The default security properties file is for linux
PROPS_SRC   = $(TOPDIR)/src/share/lib/security/java.security-linux

ifeq ($(PLATFORM), solaris)
PROPS_SRC   = $(TOPDIR)/src/share/lib/security/java.security-solaris

else # NOT_SOLARIS

# Register Microsoft CryptoAPI provider only on Windows platform.
ifeq ($(PLATFORM), windows)
  PROPS_SRC   = $(TOPDIR)/src/share/lib/security/java.security-windows

else # NOT_WINDOWS

ifeq ($(PLATFORM), macosx)
  PROPS_SRC   = $(TOPDIR)/src/share/lib/security/java.security-macosx
endif # MACOSX

endif # NOT_WINDOWS
endif # NOT_SOLARIS


PROPS_BUILD = $(LIBDIR)/security/java.security

PKCS11_CONFIG_SRC = $(TOPDIR)/src/share/lib/security/nss.cfg
PKCS11_CONFIG_BUILD = $(LIBDIR)/security/nss.cfg

POLICY_SRC =	$(TOPDIR)/src/share/lib/security/java.policy
POLICY_BUILD = $(LIBDIR)/security/java.policy

CACERTS_SRC =	$(CACERTS_FILE)
CACERTS_BUILD = $(LIBDIR)/security/cacerts

ifndef OPENJDK
  BLACKLIST_SRC =	$(CLOSED_SHARE_SRC)/lib/security/blacklist
  BLACKLIST_BUILD = $(LIBDIR)/security/blacklist
  TRUSTEDLIBS_SRC = $(CLOSED_SHARE_SRC)/lib/security/trusted.libraries
  TRUSTEDLIBS_BUILD = $(LIBDIR)/security/trusted.libraries
endif

FILES_class = $(FILES_java:%.java=$(CLASSBINDIR)/%.class)

#
# Rules
#
include $(BUILDDIR)/common/Rules.gmk

ifdef OPENJDK
build: properties pkcs11config policy cacerts
else
build: properties policy cacerts blacklist trustedlibs
endif

install: all

properties: classes  $(PROPS_BUILD)

pkcs11config: classes  $(PKCS11_CONFIG_BUILD)

policy: classes  $(POLICY_BUILD)

cacerts: classes $(CACERTS_BUILD)

blacklist: classes $(BLACKLIST_BUILD)

trustedlibs: classes $(TRUSTEDLIBS_BUILD)

$(PROPS_BUILD): $(PROPS_SRC)
	$(install-file)

$(PKCS11_CONFIG_BUILD): $(PKCS11_CONFIG_SRC)
	$(install-file)

$(POLICY_BUILD): $(POLICY_SRC)
	$(install-file)

$(CACERTS_BUILD): $(CACERTS_SRC)
	$(install-file)

$(BLACKLIST_BUILD): $(BLACKLIST_SRC)
	$(install-file)

$(TRUSTEDLIBS_BUILD): $(TRUSTEDLIBS_SRC)
	$(install-file)

clean clobber:: .delete.classlist
	$(RM) -r $(CLASSBINDIR)/java/security
	$(RM) $(PROPS_BUILD) $(PKCS11_CONFIG_BUILD) $(POLICY_BUILD) $(CACERTS_BUILD) $(BLACKLIST_BUILD) $(TRUSTEDLIBS_BUILD)

# Additional Rule for building sun.security.util
$(CLASSBINDIR)/%.class: $(SHARE_SRC)/sun/%.java
	@$(ECHO) $? >> $(TEMPDIR)/.classes.list

