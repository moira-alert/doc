BUILD_DIR = build
CLIENTS_DIR = clients
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

generate-py:
	openapi-generator generate -g python -i $(SPEC_FILE) -o $(CLIENTS_DIR)/python
generate-go:
	openapi-generator generate -g go -i $(SPEC_FILE) -o $(CLIENTS_DIR)/go
generate-ts:
	openapi-generator generate -g typescript-node -i $(SPEC_FILE) -o $(CLIENTS_DIR)/ts-node
generate-cs:
	openapi-generator generate -g csharp -i $(SPEC_FILE) -o $(CLIENTS_DIR)/csharp

