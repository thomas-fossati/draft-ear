.DEFAULT_GOAL := check

SHELL := /bin/bash

%.cbor: %.diag ; diag2cbor.rb $< > $@

include tools.mk

check:: check-ear-json check-ear-json-examples
#check:: check-ear-cbor check-ear-cbor-examples
#check:: check-ext-veraison-cbor check-ext-veraison-cbor-examples
#check:: check-ext-veraison-json check-ext-veraison-json-examples
#check:: check-ext-teep-json check-ext-teep-json-examples

# $1: label
# $2: cddl fragments
# $3: (optional) diag test files
define cddl_check_template

check-$(1): $(1)-autogen.cddl
	$$(cddl) $$< g 1 | $$(diag2diag) -e

.PHONY: check-$(1)

$(1)-autogen.cddl: $(2)
	for f in $$^ ; do ( grep -v '^;' $$$$f ; echo ) ; done > $$@

CLEANFILES += $(1)-autogen.cddl

check-$(1)-examples: $(1)-autogen.cddl $(3:.diag=.cbor)
	@for f in $(3:.diag=.cbor); do \
		echo ">> validating $$$$f" ; \
		$$(cddl) $$< validate $$$$f || exit 1; \
	done

.PHONY: check-$(1)-examples

CLEANFILES += $(3:.diag=.cbor)

endef # cddl_check_template

include ear-json-frags.mk

$(eval $(call cddl_check_template,ear-json,$(EAR_JSON_FRAGS),$(EAR_JSON_EXAMPLES)))

CBOR_FRAGS := attestation-result.cddl
CBOR_FRAGS += cbor-labels.cddl
CBOR_FRAGS += generic-non-empty.cddl

CBOR_EXAMPLES := $(wildcard examples/cbor-*.diag)

$(eval $(call cddl_check_template,cbor,$(CBOR_FRAGS),$(CBOR_EXAMPLES)))

VERAISON_CBOR_FRAGS := attestation-result.cddl
VERAISON_CBOR_FRAGS += cbor-labels.cddl
VERAISON_CBOR_FRAGS += generic-non-empty.cddl
VERAISON_CBOR_FRAGS += veraison.cddl
VERAISON_CBOR_FRAGS += veraison-cbor-labels.cddl

VERAISON_CBOR_EXAMPLES := $(wildcard examples/veraison-cbor-*.diag)

$(eval $(call cddl_check_template,veraison-cbor,$(VERAISON_CBOR_FRAGS),$(VERAISON_CBOR_EXAMPLES)))

VERAISON_JSON_FRAGS := attestation-result.cddl
VERAISON_JSON_FRAGS += json-labels.cddl
VERAISON_JSON_FRAGS += generic-non-empty.cddl
VERAISON_JSON_FRAGS += veraison.cddl
VERAISON_JSON_FRAGS += veraison-json-labels.cddl

VERAISON_JSON_EXAMPLES := $(wildcard examples/veraison-json-*.diag)

$(eval $(call cddl_check_template,veraison-json,$(VERAISON_JSON_FRAGS),$(VERAISON_JSON_EXAMPLES)))

TEEP_JSON_FRAGS := attestation-result.cddl
TEEP_JSON_FRAGS += json-labels.cddl
TEEP_JSON_FRAGS += generic-non-empty.cddl
TEEP_JSON_FRAGS += teep.cddl
TEEP_JSON_FRAGS += teep-json-labels.cddl
TEEP_JSON_FRAGS += coswid-version-scheme.cddl

TEEP_JSON_EXAMPLES := $(wildcard examples/teep-json-*.diag)

$(eval $(call cddl_check_template,teep-json,$(TEEP_JSON_FRAGS),$(TEEP_JSON_EXAMPLES)))

clean: ; rm -f $(CLEANFILES)