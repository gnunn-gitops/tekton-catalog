This task performs an image policy check using ACS and the roxctl CLI.

This image check will flag any policies that have been violated and store whether the check
passed or failed in the result `check_passed`. A failure in the check does not fail
the pipeline, it follows a philosphy of not blocking devs and thus is expected that
policy failures are handled asynchronously via notifications or pull requests.