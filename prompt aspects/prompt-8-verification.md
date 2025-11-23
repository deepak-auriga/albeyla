chmod +x tests/*.sh

# Run platform health check
./tests/verify_platform.sh
# Expected: All green checkmarks

# Run integration test
./tests/verify_integration.sh
# Expected: Success message with JSON output