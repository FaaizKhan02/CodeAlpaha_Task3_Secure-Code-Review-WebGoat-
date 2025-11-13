# 🔐 Secure Code Review Report – OWASP WebGoat (Java)

## 1️⃣ Overview
This report documents the results of a **Secure Coding Review** performed on the **OWASP WebGoat** Java application.  
The objective was to perform a **static code analysis** using the **SpotBugs** tool to identify common coding vulnerabilities, assess potential risks, and recommend secure development practices to improve the security posture of the application.

---

## 2️⃣ Tools and Environment
| **Tool / Component** | **Purpose** |
|----------------------|-------------|
| **OWASP WebGoat** | A deliberately insecure Java web application used for teaching application security concepts. |
| **Java (OpenJDK 17)** | Programming language used to build and run WebGoat. |
| **Maven** | Build automation tool for compiling and managing dependencies. |
| **SpotBugs** | Static code analysis tool used to detect potential bugs and security vulnerabilities in Java code. |
| **Operating System** | Kali Linux 2024 |

---

## 3️⃣ Methodology
The following steps were performed to conduct the secure code review:

### **Step 1: Setup**
1. Installed required dependencies:
   ```bash
   sudo apt update
   sudo apt install openjdk-17-jdk maven -y

    Cloned the OWASP WebGoat project:

    git clone https://github.com/WebGoat/WebGoat.git
    cd WebGoat/WebGoat

Step 2: Build the Application

Compiled the project using Maven:

mvn clean package -DskipTests

This generated the required compiled classes in the /target folder.
Step 3: Static Analysis with SpotBugs

Executed SpotBugs to analyze the compiled bytecode:

mvn com.github.spotbugs:spotbugs-maven-plugin:4.7.3.0:spotbugs -DspotbugsXmlOutput=true -DspotbugsOutputDirectory=target

This produced an XML report:

/WebGoat/target/spotbugsXml.xml

Step 4: Review of Results

The spotbugsXml.xml file was inspected to identify all BugInstance entries, which represent potential coding or security issues.
Each issue was classified according to its severity, type, and affected component.
4️⃣ Findings Summary
#	File / Class	Vulnerability Type	Severity	Description	Recommendation
1	AsciiDoctorTemplateResolver.java	Reliance on Default Encoding	High	The code uses the system’s default character encoding, which can lead to inconsistent behavior and potential data corruption.	Always specify a charset, e.g., StandardCharsets.UTF_8, when reading or writing files.
2	VulnerableTaskHolder.java	Resource Leak	Medium	Input streams are opened but not properly closed, leading to potential memory or file handle leaks.	Use try-with-resources (try (InputStream in = ...) {}) to automatically close resources.
3	AttackResult.java	Mutable Object Exposure	Medium	Arrays and collections are returned directly, allowing modification of internal data from outside the class.	Return cloned copies of mutable objects or use unmodifiable wrappers.
4	EnvironmentExposure.java	Static Field Written by Instance Method	High	Instance methods modify static fields, which can cause data corruption and thread-safety issues.	Synchronize access or redesign to avoid modifying static fields.
5	DatabaseConfiguration.java	Internal Representation Exposure	Medium	Mutable configuration objects are shared without defensive copies.	Always clone configuration objects before returning them.
6	WebSecurityConfig.java	Mutable Object Return	Medium	A shared service instance is returned, which external classes can modify.	Use dependency injection safely and avoid exposing shared mutable states.
5️⃣ Risk Analysis Summary
Category	Issues Found	Risk Level
Encoding Vulnerabilities	1	High
Resource Leaks	1	Medium
Mutable Object Exposure	3	Medium
Static Field Access	1	High

Total Issues: 6
Overall Risk: 🔴 Medium to High
6️⃣ Secure Coding Recommendations

    Always define explicit character encodings

        Use new InputStreamReader(in, StandardCharsets.UTF_8) instead of relying on system defaults.

    Ensure proper resource management

        Apply try-with-resources or explicit close() in finally blocks for streams, readers, and sockets.

    Avoid mutable shared states

        Use immutable data structures or return defensive copies to prevent unintended data modifications.

    Eliminate unsafe static field usage

        Avoid writing to shared static fields in multithreaded environments; use synchronized access if necessary.

    Validate and sanitize inputs

        Though not directly flagged by SpotBugs, manual inspection recommends adding input validation to reduce injection risks.

7️⃣ Remediation Plan
Vulnerability	Remediation Action	Example Fix
Default Encoding	Explicitly specify encoding in all I/O operations.	new InputStreamReader(file, StandardCharsets.UTF_8)
Resource Leak	Use try-with-resources block.	try (FileInputStream fis = new FileInputStream(file)) {}
Mutable Object Exposure	Return copies of arrays or collections.	return Arrays.copyOf(feedbackArgs, feedbackArgs.length);
Static Field Write	Remove static field mutation.	Convert to instance variable or add synchronization.
8️⃣ Results and Observations

    SpotBugs effectively identified multiple code-level weaknesses that can lead to security issues.

    Common problems in WebGoat’s codebase relate to thread-safety, data immutability, and resource handling.

    These issues align with OWASP’s recommended secure coding categories, such as Error Handling, Data Validation, and System Configuration.

9️⃣ Conclusion

The OWASP WebGoat Java application demonstrates several typical coding flaws found in web applications.
By applying the recommended secure coding practices, developers can improve both the security and reliability of the application.
This exercise reinforced the importance of static analysis tools like SpotBugs in identifying vulnerabilities early in the software development lifecycle.
🔧 Tools Summary
Tool	Purpose
SpotBugs	Static analysis tool for bug and vulnerability detection
Maven	Build automation and dependency management
OpenJDK 17	Java runtime and compiler
Kali Linux	Analysis environment
GitHub	Documentation and version control platform
✍️ Author

Name: Faaiz Khan
Task: Secure Coding Review (OWASP WebGoat – Java)
Platform: Kali Linux
Tools: Java | Maven | SpotBugs | OWASP WebGoat
#CyberSecurity #SecureCoding #OWASP #SpotBugs #JavaSecurity #AppSec
