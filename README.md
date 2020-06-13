### Demo project for spring cloud contract verifier xml bug

This is a repository to show that spring cloud contract has a bug when generating the code to test xml contracts with namespaces.

## Repository

The repository is confomed by a contract ```contracts/example.yml``` which has a contract with a response body which is an xml which has a name space. A```docker-compose.yml``` file that starts a mock service in mountebank and runs the spring test runner container.

## How to reproduce
Just run
```
docker-compose up -d
```

# Expected result
The tests pass and the generated tests:
```
build/spring-cloud-contract-output/contracts/contracts/ContractVerifierTest.java
```
validate the xml contract.
```java
assertThat(valueFromXPath(parsedXml, "/customer/email/text()")).isEqualTo("customer@test.com");
```

# Actual result
The test pass but the generate test validation is wrong
```java
assertThat(valueFromXPath(parsedXml, "/ns1:customer/email/text()")).isEqualTo("");
```

Also removing the namespaces from the xml fixes the problem.