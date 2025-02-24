#
# %CopyrightBegin%
#
# Copyright Ericsson AB 1999-2021. All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
# %CopyrightEnd%
#
#
include $(ERL_TOP)/make/target.mk
include $(ERL_TOP)/make/$(TARGET)/otp.mk

# ----------------------------------------------------
# Application version
# ----------------------------------------------------
include $(ERL_TOP)/erts/vsn.mk

APPLICATION=otp-system-documentation

# ----------------------------------------------------
# Release directory specification
# ----------------------------------------------------
RELSYSDIR = $(RELEASE_PATH)/doc

GIF_FILES = 

INFO_FILES = ../../../README.md ../../COPYRIGHT PR.template

TOPDOCDIR=.

include ../general_info/xmlfiles.mk
include ../installation_guide/xmlfiles.mk
include ../system_principles/xmlfiles.mk
include ../embedded/xmlfiles.mk
include ../getting_started/xmlfiles.mk
include ../reference_manual/xmlfiles.mk
include ../programming_examples/xmlfiles.mk
include ../efficiency_guide/xmlfiles.mk
include ../tutorial/xmlfiles.mk
include ../design_principles/xmlfiles.mk
include ../oam/xmlfiles.mk

BOOK_FILES = book.xml

XML_FILES = \
	$(BOOK_FILES)

XML_GUIDE_FILES = \
	$(GENERAL_INFO_CHAPTER_FILES:%=general_info/%) \
	$(GENERAL_INFO_CHAPTER_GEN_FILES:%=general_info/%) \
	$(INST_GUIDE_CHAPTER_FILES:%=installation_guide/%) \
	$(INST_GUIDE_CHAPTER_GEN_FILES:%=installation_guide/%) \
	$(SYSTEM_PRINCIPLES_CHAPTER_FILES:%=system_principles/%) \
	$(SYSTEM_PRINCIPLES_CHAPTER_GEN_FILES:%=system_principles/%) \
	$(EMBEDDED_CHAPTER_FILES:%=embedded/%) \
	$(EMBEDDED_CHAPTER_GEN_FILES:%=embedded/%) \
	$(GETTING_STARTED_CHAPTER_FILES:%=getting_started/%) \
	$(GETTING_STARTED_CHAPTER_GEN_FILES:%=getting_started/%) \
	$(REF_MAN_CHAPTER_FILES:%=reference_manual/%) \
	$(REF_MAN_CHAPTER_GEN_FILES:%=reference_manual/%) \
	$(PROG_EX_CHAPTER_FILES:%=programming_examples/%) \
	$(PROG_EX_CHAPTER_GEN_FILES:%=programming_examples/%) \
	$(EFF_GUIDE_CHAPTER_FILES:%=efficiency_guide/%) \
	$(EFF_GUIDE_CHAPTER_GEN_FILES:%=efficiency_guide/%) \
	$(TUTORIAL_CHAPTER_FILES:%=tutorial/%) \
	$(TUTORIAL_CHAPTER_GEN_FILES:%=tutorial/%) \
	$(DESIGN_PRINCIPLES_CHAPTER_FILES:%=design_principles/%) \
	$(DESIGN_PRINCIPLES_CHAPTER_GEN_FILES:%=design_principles/%) \
	$(OAM_CHAPTER_FILES:%=oam/%) \
	$(OAM_CHAPTER_GEN_FILES:%=oam/%)

XML_GEN_FILES = \
	$(XML_GUIDE_FILES:%=$(XMLDIR)/%) \
	$(XMLDIR)/general_info/part.xml \
	$(XMLDIR)/installation_guide/part.xml \
	$(XMLDIR)/system_principles/part.xml \
	$(XMLDIR)/embedded/part.xml \
	$(XMLDIR)/getting_started/part.xml \
	$(XMLDIR)/reference_manual/part.xml \
	$(XMLDIR)/programming_examples/part.xml \
	$(XMLDIR)/efficiency_guide/part.xml \
	$(XMLDIR)/tutorial/part.xml \
	$(XMLDIR)/design_principles/part.xml \
	$(XMLDIR)/oam/part.xml


XMLLINT_SRCDIRS= $(XMLDIR)/general_info:$(XMLDIR)/installation_guide:$(XMLDIR)/system_principles:$(XMLDIR)/embedded:$(XMLDIR)/getting_started:$(XMLDIR)/reference_manual:$(XMLDIR)/programming_examples:$(XMLDIR)/efficiency_guide:$(XMLDIR)/tutorial:$(XMLDIR)/design_principles:$(XMLDIR)/oam
HTMLDIR= ../html
PDFREFDIR= pdf

TOP_PDF_FILE = $(PDFDIR)/$(APPLICATION)-$(VSN).pdf
TOPDOC=true

sp :=

sp +=

## qs translates ' ' to '\ ', sq translates '\ ' to ' '
## These function are used when the make target is a path that can
## contain spaces. This is needed a the target 'foo bar:' needs to be
## written as 'foo\ bar:' in order for make to interpret it as a single
## target. Unfortunately, when this is done $@ will contain '\ ', which
## means that we have to use sq to remove the escape again. A small example
## looks like this:
##
## $(call qs, /path/with space/file.o): $(call qs, /path/with space/file.c)
##   	gcc -c -o "$(call sq, $@)" "$(call sq, $^)"
qs = $(subst $(sp),\$(sp),$1)
sq = $(subst \$(sp),$(sp),$1)

ifdef RELEASE_PATH
INST_TYPE=rel
INST_TYPE_SRC_DIR=$(RELEASE_PATH)
# We build to the 'temporary' dir in order to be able to install
# results using INSTALL_DATA (in order to get correct access
# rights on installed files)
# The temporary folder needs to be in the released system as we do
# not know if we have write permissions in the source release.
INST_TYPE_DEST_DIR=$(RELSYSDIR)/temporary
INST_TYPE_DEST_DIR_DEP=$(call qs,$(INST_TYPE_DEST_DIR))
INST_TYPE_JS_DEST_DIR=$(INST_TYPE_DEST_DIR)
INST_TYPE_VSN_FILE=$(INST_TYPE_DEST_DIR)/OTP_VERSION
else
INST_TYPE=src
INST_TYPE_SRC_DIR=$(ERL_TOP)
INST_TYPE_DEST_DIR=$(HTMLDIR)
INST_TYPE_DEST_DIR_DEP=
INST_TYPE_JS_DEST_DIR=$(INST_TYPE_DEST_DIR)/js
INST_TYPE_VSN_FILE=$(ERL_TOP)/OTP_VERSION
endif

#--------------------------------------------------------------------------
# We generate the index page from the installed system. This make
# it important that this is done last. The file index.html.src
# is used as a template for the resulting page.

EBIN = ebin

INDEX_SCRIPT = $(EBIN)/erl_html_tools.$(EMULATOR)
INDEX_SRC = src/erl_html_tools.erl

INDEX_HTML=$(call qs,$(INST_TYPE_DEST_DIR)/index.html)
APPLICATIONS_HTML=$(call qs,$(INST_TYPE_DEST_DIR)/applications.html)
INDEX_FILES = $(INDEX_HTML) $(APPLICATIONS_HTML)

JAVASCRIPT = $(call qs,$(INST_TYPE_JS_DEST_DIR)/erlresolvelinks.js)
JAVASCRIPT_BUILD_SCRIPT = $(EBIN)/erlresolvelinks.$(EMULATOR)
JAVASCRIPT_BUILD_SCRIPT_SRC = src/erlresolvelinks.erl

MAN_INDEX_SCRIPT = $(EBIN)/otp_man_index.$(EMULATOR)
MAN_INDEX_SRC = src/otp_man_index.erl
MAN_INDEX = $(call qs,$(INST_TYPE_DEST_DIR)/man_index.html)

GLOSSARY  = $(HTMLDIR)/glossary.html
GLOSSARY_SRC = $(ERL_TOP)/system/internal_tools/doctools/src/glossary.erl
GLOSSARY_SCRIPT = $(EBIN)/glossary.$(EMULATOR)

TEMPLATES = \
	templates/index.html.src \
	templates/applications.html.src

#--------------------------------------------------------------------------

$(INDEX_SCRIPT): $(INDEX_SRC)
	$(ERLC) -o$(EBIN) +warn_unused_vars $<

$(call qs,$(INST_TYPE_DEST_DIR)/OTP_VERSION): $(INST_TYPE_DEST_DIR_DEP)
	if test -f "$(RELEASE_PATH)/releases/$(SYSTEM_VSN)/OTP_VERSION"; then \
		$(CP) "$(RELEASE_PATH)/releases/$(SYSTEM_VSN)/OTP_VERSION" "$(call sq,$@)"; \
	else \
		$(CP) "$(ERL_TOP)/OTP_VERSION" "$(call sq,$@)"; \
	fi

# We don't list toc_*.html as targets because we don't know
$(INDEX_HTML) + $(APPLICATIONS_HTML): $(INST_TYPE_DEST_DIR_DEP) $(INDEX_SCRIPT) $(TEMPLATES) $(call qs,$(INST_TYPE_VSN_FILE))
	echo "Generating index $@"
	$(ERL) +pc unicode -noshell -pa $(EBIN) -s erl_html_tools top_index $(INST_TYPE) \
		"$(INST_TYPE_SRC_DIR)" "$(INST_TYPE_DEST_DIR)" \
		`cat "$(INST_TYPE_VSN_FILE)"` -s erlang halt


#--------------------------------------------------------------------------

$(JAVASCRIPT_BUILD_SCRIPT): $(JAVASCRIPT_BUILD_SCRIPT_SRC)
	$(ERLC) -o$(EBIN) +warn_unused_vars $<

$(JAVASCRIPT): $(INST_TYPE_DEST_DIR_DEP) $(JAVASCRIPT_BUILD_SCRIPT)
	$(ERL) +pc unicode -noshell -pa $(EBIN) -run erlresolvelinks make $(ERL_TOP) \
		"$(INST_TYPE_SRC_DIR)" "$(INST_TYPE_JS_DEST_DIR)" -s erlang halt

#--------------------------------------------------------------------------

$(MAN_INDEX_SCRIPT): $(MAN_INDEX_SRC)
	$(ERLC) -o$(EBIN) +warn_unused_vars $<

$(MAN_INDEX): $(INST_TYPE_DEST_DIR_DEP) $(MAN_INDEX_SCRIPT)
	$(ERL) +pc unicode -noshell -pa $(EBIN) -s otp_man_index gen $(INST_TYPE) \
		"$(INST_TYPE_SRC_DIR)" "$(call sq,$@)" -s erlang halt

#--------------------------------------------------------------------------

$(HTMLDIR)/highlights.html:  highlights.xml
	date=`date +"%B %e, %Y"`; \
	$(XSLTPROC) --output $(@) \
		--stringparam docgen "$(DOCGEN)" \
		--stringparam topdocdir "$(TOPDOCDIR)" \
		--stringparam pdfdir "$(PDFREFDIR)"  \
		--stringparam gendate "$$date" \
		--stringparam appname "$(APPLICATION)" \
		--stringparam appver "$(VSN)" \
		--stringparam stylesheet "$(CSS_FILE)" \
		--stringparam winprefix "$(WINPREFIX)" \
		--stringparam logo "$(HTMLLOGO_FILE)" \
		-path  $(DOCGEN)/priv/dtd \
		-path  $(DOCGEN)/priv/dtd_html_entities \
		$(DOCGEN)/priv/xsl/db_html.xsl $<


$(HTMLDIR)/incompatible.html:  incompatible.xml
	date=`date +"%B %e, %Y"`; \
	$(XSLTPROC) --output $(@) --stringparam docgen "$(DOCGEN)" \
		--stringparam topdocdir "$(TOPDOCDIR)" \
		--stringparam pdfdir "$(PDFREFDIR)"  \
		--stringparam gendate "$$date" \
		--stringparam appname "$(APPLICATION)" \
		--stringparam appver "$(VSN)" \
		--stringparam stylesheet "$(CSS_FILE)" \
		--stringparam winprefix "$(WINPREFIX)" \
		--stringparam logo "$(HTMLLOGO_FILE)" \
		-path  $(DOCGEN)/priv/dtd \
		-path  $(DOCGEN)/priv/dtd_html_entities \
		$(DOCGEN)/priv/xsl/db_html.xsl $<

#--------------------------------------------------------------------------

$(GLOSSARY_SCRIPT): $(GLOSSARY_SRC)
	$(ERLC) -o$(EBIN) $(GLOSSARY_SRC)

$(GLOSSARY): $(GLOSSARY_SCRIPT)
	$(ERL) -noshell -pa $(EBIN) \
		-s glossary make ../definitions/term.defs -s erlang halt > \
		$(GLOSSARY)

#--------------------------------------------------------------------------

PR.template: PR.template.src $(ERL_TOP)/make/$(TARGET)/otp.mk
	sed -e 's;%VSN%;$(VSN);' \
	    -e 's;%SYSTEM_VSN%;$(SYSTEM_VSN);' \
	    $< > $@

# ----------------------------------------------------
# FLAGS 
# ----------------------------------------------------
XML_FLAGS += 
DVIPS_FLAGS += 

# ----------------------------------------------------
# Targets
# ----------------------------------------------------

DOC_TARGETS?=pdf html man

docs: $(DOC_TARGETS) $(INFO_FILES)

local_docs: PDFREFDIR=../pdf

$(TOP_PDF_FILE): $(XML_FILES)

pdf: $(TOP_PDF_FILE)

html: $(INDEX_FILES) $(JAVASCRIPT) $(MAN_INDEX)

man:

debug opt:

clean:
	$(RM) ../html/js/*.js
	$(RM) PR.template
	$(RM) $(XMLDIR)/*.xml
	$(RM) $(INDEX_FILES) $(MAN_INDEX)
	$(RM) $(TOP_PDF_FILE) $(TOP_PDF_FILE:%.pdf=%.fo)
	$(RM) $(INDEX_SCRIPT) $(GLOSSARY_SCRIPT) $(JAVASCRIPT_BUILD_SCRIPT) 
	$(RM) erl_crash.dump errs core *~

# ----------------------------------------------------
# Release Target
# ---------------------------------------------------- 
include $(ERL_TOP)/make/otp_release_targets.mk

$(call qs,$(RELSYSDIR)/temporary):
	$(INSTALL_DIR) "$(RELSYSDIR)/temporary"

$(call qs,$(RELSYSDIR)/docbuild):
	$(INSTALL_DIR) "$(RELSYSDIR)/docbuild"

release_man_spec: man $(call qs,$(RELSYSDIR)/docbuild)

release_html_spec: html $(call qs,$(RELSYSDIR)/docbuild)
	$(INSTALL_DATA) $(MAN_INDEX) "$(RELSYSDIR)"
	$(INSTALL_DATA) $(MAN_INDEX_SRC) $(MAN_INDEX_SCRIPT) "$(RELSYSDIR)/docbuild"
	$(INSTALL_DIR)  "$(RELSYSDIR)/js"
	$(INSTALL_DATA) $(JAVASCRIPT) "$(RELSYSDIR)/js"
	$(INSTALL_DATA) $(INDEX_FILES) "$(RELSYSDIR)"
	$(INSTALL_DATA) $(INDEX_SCRIPT) $(JAVASCRIPT_BUILD_SCRIPT) \
		$(INDEX_SRC) $(JAVASCRIPT_BUILD_SCRIPT_SRC) \
		$(TEMPLATES) "$(RELSYSDIR)/docbuild"

release_pdf_spec: pdf
	$(INSTALL_DIR)  "$(RELSYSDIR)/pdf"
	$(INSTALL_DATA)  \
		$(TOP_PDF_FILE) "$(RELSYSDIR)/pdf"

release_docs_spec: $(DOC_TARGETS:%=release_%_spec) $(INFO_FILES)
	$(INSTALL_DATA) $(INFO_FILES) "$(RELEASE_PATH)"
	$(RM) -r "$(RELSYSDIR)/temporary"

release_spec:
