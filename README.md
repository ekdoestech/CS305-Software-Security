# CS 305: Software Security

## Artemis Financial Secure Software Project

This repository contains a portfolio artifact from CS 305: Software Security. The project involved assessing and refactoring a Java Spring Boot application for Artemis Financial. The work demonstrates secure coding practices, cryptographic hashing, certificate generation, HTTPS configuration, dependency analysis, vulnerability remediation, and functional testing.

## Portfolio Artifact

[View the Artemis Financial Practices for Secure Software Report](./Erica_Kinch_Artemis_Financial_Secure_Software_Report.pdf)

## Reflection

### Client and Software Requirements

Artemis Financial is a financial-planning company that develops individualized financial plans for its customers. The company wanted to modernize its web application while protecting sensitive customer and financial information. The application needed a secure communication channel, a method for verifying data integrity, and an assessment of its third-party dependencies.

I refactored the supplied Java Spring Boot application by adding a SHA-256 checksum endpoint, generating a self-signed X.509 certificate for local testing, and configuring the application to use HTTPS on port 8443. I also evaluated the project’s dependencies and addressed the vulnerabilities identified during the initial assessment.

### Security Assessment and Business Value

One area I handled particularly well was connecting the automated scan results to the actual application. The initial OWASP Dependency-Check scan evaluated 49 dependencies and reported 15 vulnerable dependencies with 242 findings. I updated the Spring Boot version, moved the project to Java 17, removed an unused Spring Data REST dependency, and updated components such as Tomcat and Log4j. The final scan evaluated 39 dependencies and reported no active vulnerable dependencies.

Secure coding is important because financial applications handle information that could cause serious harm if it were exposed or modified. Security weaknesses can lead to unauthorized access, loss of customer trust, service interruptions, recovery expenses, and legal or regulatory concerns. Building security into the application also makes the software easier to maintain because developers can identify risks before they reach production.

### Challenges and Lessons Learned

The most challenging part of the project was interpreting the Dependency-Check results. A vulnerability scanner can identify a possible match between a dependency and a published CVE, but that does not automatically mean the vulnerable feature is being used by the application. The remaining Tomcat finding concerned an optional WebSocket chat example that was not included in this project. After confirming that the affected component was not present, I documented the finding and narrowly suppressed it.

This process taught me that secure development requires more than reporting the number of scanner warnings. Developers must examine the affected component, understand the conditions required to exploit the vulnerability, and document why a finding is remediated, accepted, or not applicable. I also gained a clearer understanding of the difference between hashing and encryption. SHA-256 was appropriate for verifying data integrity, while TLS protected information transmitted between the browser and server.

### Layered Security and Future Assessment

I increased the application’s security in several layers. The SHA-256 checksum provides an integrity check for data. The X.509 certificate and RSA key pair support server identification during the local demonstration. HTTPS uses TLS to protect data in transit. Dependency updates reduced exposure to known third-party vulnerabilities, while manual review and functional testing helped identify problems that an automated scanner might miss.

For a future application, I would begin by conducting threat modeling and reviewing the application’s data flows, trust boundaries, entry points, and authentication requirements. I would combine manual code review with software composition analysis, static application security testing, and targeted functional tests. Scanner results would be prioritized according to severity, exploitability, affected functionality, and the application’s business context. I would also incorporate security checks into the continuous integration pipeline to identify new vulnerable dependencies or insecure changes early.

### Functional and Security Verification

After refactoring the application, I used Maven to build and test the project and received a successful build. I then started the application through HTTPS and accessed the `/hash` endpoint. The endpoint returned the expected static data and a 64-character SHA-256 checksum.

I reran OWASP Dependency-Check after making the dependency changes and compared the results with the initial report. I also reviewed the code for syntax, logical flow, and security concerns. These steps confirmed that the application remained functional and that the changes did not introduce additional reported vulnerabilities. In a production environment, I would also move the keystore password out of `application.properties` and retrieve it from an environment variable or secrets-management service.

### Resources, Tools, and Coding Practices

The most useful resources and tools in this project were Java’s `MessageDigest` class, `StandardCharsets.UTF_8`, Java `keytool`, a PKCS12 keystore, Spring Boot configuration, Maven, and OWASP Dependency-Check. NIST guidance, OWASP resources, CVE records, and Spring documentation were also useful when evaluating cryptographic choices and dependency findings.

Practices I would use again include selecting cryptographic functions for their intended purpose, explicitly defining text encoding before hashing, keeping dependencies up to date, removing unused components, documenting vulnerability decisions, and retesting after every security-related change. I would also avoid committing credentials, private keys, or other secrets to a public repository.

### Skills Demonstrated

This artifact demonstrates my ability to assess software for security weaknesses, interpret dependency-scan results, update vulnerable components, and verify that remediation does not break the application. It also shows that I can implement a cryptographic checksum, configure certificates and TLS, investigate a possible false positive, and document security decisions.

For a future employer, the project provides evidence that I understand security as part of the complete software development lifecycle. I can evaluate risks, select appropriate controls, apply secure coding practices, preserve application functionality, and explain the reasoning behind my decisions.
