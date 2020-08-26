BUILD_DIR = build
SPEC_FILE = $(BUILD_DIR)/openapi.yml
TESTS_LOG_FILE  = $(BUILD_DIR)/cassette.yml
API_URL = "http://localhost:8080/api"

spec:
	swagger-cli bundle main.yml -o $(SPEC_FILE) -t yaml

validate-spec:
	openapi-generator validate -i $(SPEC_FILE)

test-spec:
	schemathesis run \
	--base-url=$(API_URL) $(SPEC_FILE) \
	--checks all \
 	--store-network-log $(TESTS_LOG_FILE) \
 	--show-errors-tracebacks \
 	--hypothesis-verbosity verbose \
 	--hypothesis-max-examples 1 \
