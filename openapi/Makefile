BUILD_DIR = build
SPEC_FILE = $(BUILD_DIR)/openapi.yml

spec:
	swagger-cli bundle main.yml -o $(SPEC_FILE) -t yaml

validate-spec:
	openapi-generator validate -i $(SPEC_FILE)

