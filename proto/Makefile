GOOGLEAPIS := ${HOME}/src/3p/googleapis
PROTOBUF := ${HOME}/src/3p/protobuf/src

PROTOC = protoc
PROTOC_FLAGS = -I . -I $(GOOGLEAPIS) -I $(PROTOBUF)

pbgo_out := ../go/gen/proto

REMOTESAPI_protos := \
  dolt/services/remotesapi/v1alpha1/chunkstore.proto \
  dolt/services/remotesapi/v1alpha1/credentials.proto
REMOTESAPI_pbgo_pkg_path := dolt/services/remotesapi_v1alpha1

PBGO_pkgs := REMOTESAPI

all:

.PHONY: clean

clean:
	@rm -f $(ALL_OUTPUTS)

define PROTOC_template # 1=proto, 2=outputs, 3=flags
$(2) : $(1)
	@$$(PROTOC) $$(PROTOC_FLAGS) $(3) $(1)
ALL_OUTPUTS += $(2)
all: $(2)
endef

define PBGO_output # 1=proto
$(patsubst %.proto,%.pb.go,$(1))
endef

define PBGO_template # 1=proto, 2=output pkg path, 3=output base
$(call PROTOC_template,\
  $(1),\
  $(addprefix $(3)/$(2)/,$(notdir $(call PBGO_output,$(1)))),\
	--go_out=plugins=grpc:$(3))
endef

$(foreach p,$(PBGO_pkgs),\
  $(foreach f,$($(p)_protos),\
    $(eval \
      $(call PBGO_template,$(f),$($(p)_pbgo_pkg_path),$(pbgo_out)))))