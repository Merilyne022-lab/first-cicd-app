# CI/CD Learning Journey: Jenkins + GitHub + SonarQube in Docker

## 1. Project Goal

Build a CI/CD pipeline using Jenkins that automatically checks out code from GitHub, compiles it, and runs static code quality analysis through SonarQube — all running in a self-hosted Docker environment.

## 2. Phase 1 — Initial Setup & Credential Basics

I set up Jenkins and SonarQube as separate Docker containers and wrote a basic pipeline with three stages: Checkout, Build, and SonarQube Analysis.

My first real obstacle was getting SonarQube authentication working at all. My first attempt at storing a token in Jenkins accidentally captured raw webpage text instead of an actual token, so I learned the fundamentals of Jenkins Credentials system (Secret Text type) and how withCredentials injects a token into a pipeline as an environment variable.

## 3. Phase 2 — Docker Networking

Once the credential existed, the pipeline couldn't reach SonarQube at all. This taught me a core Docker concept: containers don't share localhost with each other or with the host machine.

I learned to inspect Docker networks, confirmed both containers were on the same custom network, and fixed the pipeline to reference SonarQube by its container name rather than localhost.

## 4. Phase 3 — Scanner/Server Version Compatibility

With networking fixed, authentication still failed with a generic "Not authorized" error — even though I verified that the token itself was valid.

Researching this led me to discover that my SonarScanner CLI version was several years older than my SonarQube server version, and recent SonarQube releases require much newer scanner versions.

I learned how to manually download, extract, and reference a specific scanner binary inside the Jenkins container, and update the pipeline path accordingly.

## 5. Phase 4 — Deep Debugging with Hashing

Even after upgrading the scanner, a new and more specific 401 error appeared, this time on SonarQube's internal bootstrap API. To rule out whether Jenkins was actually sending the token I thought it was, I learned to use SHA-256 hashing as a debugging technique — comparing the hash of my known-good token against the hash of whatever Jenkins' pipeline was actually using internally, without ever exposing the secret itself in logs. This showed a value mismatch, meaning Jenkins was using a different, stale credential value than expected.

## 6. Phase 5 — Finding the Root Cause

Through further hash comparisons and inspecting Jenkins credential usage tracking, I discovered the real root cause: my pipeline's withSonarQubeEnv step was referencing a different, pre-existing credential in Jenkins' global configuration than the one I kept editing. No matter how many times I "updated" the credential I thought was in use, Jenkins kept pulling from this other, mismatched one.

The fix was to create a brand-new, uniquely-named credential and explicitly rewire the pipeline to reference it directly — removing all ambiguity.

## 7. Phase 6 — Success

With the correct scanner version and correctly-wired credential in place, build #33 completed a full, successful SonarQube analysis: code checked out, compiled, scanned (Java and XML files), and the report uploaded to the SonarQube dashboard.

## 8. Key Skills Demonstrated

- Jenkins pipeline scripting
- Docker container networking fundamentals
- Reading and interpreting Jenkins console logs and stack traces
- Systematic debugging methodology
- Using cryptographic hashing as a safe way to debug secrets without exposing them
- Patience and persistence through 32 failed builds before reaching a working pipeline

## 9. Why This Was an Important Learning Experience

This project did not just teach me how to write a pipeline. It taught me how to debug a real multi-component system.

I had to understand:
- how Jenkins injects secrets
- how Docker networking works
- how scanner/server compatibility affects reliability
- how configuration mismatches can look like code problems
- how to isolate root causes instead of guessing

This made me think like a DevOps engineer, not just someone following a tutorial.

## 10. Final Reflection

This was a hands-on learning project that helped me connect theory to practice.

The biggest lesson was that CI/CD systems are layered and interconnected. If one component fails — credentials, network, version, or config — the entire pipeline can fail, even when the code itself is fine.

By learning how to diagnose and fix each layer, I developed a much stronger understanding of DevOps, automation, and software delivery.

## 11. Conclusion

The final build was a success, and the project proved that the pipeline could:
- pull code from GitHub
- compile Java code with Maven
- analyze the project through SonarQube
- upload the results to the dashboard
- complete the full workflow successfully

This is the foundation for future improvement in CI/CD pipelines and deployment automation.

# End of Learning Journey
