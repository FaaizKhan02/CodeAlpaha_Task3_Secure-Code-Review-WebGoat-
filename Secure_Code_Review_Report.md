# 🔐 Secure Code Review Report – OWASP WebGoat (Java)

---

## 1️⃣ Objective

The purpose of this task was to perform a **Secure Coding Review** of a Java web application to identify vulnerabilities, insecure coding practices, and suggest remediation steps based on OWASP secure coding principles.

The selected application for this review was **OWASP WebGoat** — an intentionally insecure Java-based web application designed for security training.

The analysis was done using **SpotBugs**, a static code analysis tool, to automatically detect security issues in the source code.

---

## 2️⃣ Tools and Technologies Used

| **Tool / Component** | **Purpose** |
|----------------------|-------------|
| **OWASP WebGoat** | Java web application used for secure coding training |
| **Java (OpenJDK 17)** | Programming language and runtime environment |
| **Maven** | Build automation and dependency management |
| **SpotBugs** | Static analysis tool for finding code-level vulnerabilities |
| **Kali Linux** | Operating system environment for analysis |
| **GitHub** | Used for documentation and project version control |

---

## 3️⃣ Step-by-Step Process to Reach the Results

Below are all the steps performed to set up, analyze, and generate the secure code review results.

---

### **Step 1: Environment Setup**

1. **Install required tools**
   ```bash
   sudo apt update
   sudo apt install openjdk-17-jdk maven git -y

    Verify installations

java -version
mvn -v
git --version

Create a working directory

    mkdir WebGoatReview
    cd WebGoatReview

Step 2: Download OWASP WebGoat

Clone the official OWASP WebGoat repository:

git clone https://github.com/WebGoat/WebGoat.git
cd WebGoat/WebGoat

Step 3: Build the WebGoat Application

Compile and package the Java project using Maven:

mvn clean package -DskipTests

✅ Result:
This step generated compiled classes and packaged the application JAR file in the /target folder.
A message [INFO] BUILD SUCCESS confirmed that the build was successful.
Step 4: Run Static Code Analysis using SpotBugs

Execute SpotBugs using the Maven plugin:

mvn com.github.spotbugs:spotbugs-maven-plugin:4.7.3.0:spotbugs \
  -DspotbugsXmlOutput=true \
  -DspotbugsOutputDirectory=target

✅ Result:
SpotBugs performed a complete static analysis of all Java classes in the project and generated the XML report:

/WebGoat/target/spotbugsXml.xml

Step 5: Locate and Verify the Report

Check the target directory for the generated XML file:

ls target/

✅ Output:

spotbugsXml.xml

This XML file contains detailed information about each detected vulnerability, including the class name, line number, severity, and a short description.
Step 6: Analyze the Findings

The XML report was manually reviewed using:

less target/spotbugsXml.xml

Each <BugInstance> tag corresponds to a security or code-quality issue detected by SpotBugs.

These were categorized based on severity (High, Medium, Low) and grouped into security categories.
4️⃣ Findings Summary
#	File / Class	Vulnerability Type	Severity	Description	Recommendation
1	AsciiDoctorTemplateResolver.java	Reliance on Default Encoding	High	Code uses the system’s default character encoding, which can lead to inconsistent data behavior and injection risks.	Always specify StandardCharsets.UTF_8 when reading/writing files.
2	VulnerableTaskHolder.java	Resource Leak	Medium	Input/output streams are not closed properly, leading to resource exhaustion.	Use try-with-resources or close streams in a finally block.
3	AttackResult.java	Mutable Object Exposure	Medium	Mutable arrays are returned directly, allowing modification of internal data.	Return cloned arrays or use immutable data wrappers.
4	EnvironmentExposure.java	Static Field Written by Instance Method	High	Instance methods modify static fields, which can cause data corruption in concurrent execution.	Use synchronized access or redesign to avoid shared mutable static fields.
5	DatabaseConfiguration.java	Internal Representation Exposure	Medium	Mutable configuration objects are exposed externally.	Return deep copies or immutable objects to maintain data integrity.
6	WebSecurityConfig.java	Mutable Object Return	Medium	Returns modifiable service object, which may be altered by external code.	Use dependency injection safely and make objects immutable.
5️⃣ Risk Assessment
Category	Issues Found	Risk Level
Encoding Vulnerabilities	1	High
Resource Management	1	Medium
Mutable Object Exposure	3	Medium
Static Field Access	1	High

Total Issues: 6
Overall Risk Rating: 🔴 Medium to High
6️⃣ Secure Coding Best Practices & Recommendations

    Explicit Encoding:

        Always specify character encodings in I/O operations:

        new InputStreamReader(fileInputStream, StandardCharsets.UTF_8);

    Proper Resource Management:

        Use try-with-resources to automatically close files and streams.

    Immutable Design:

        Avoid returning or exposing mutable data structures (arrays, lists, maps).

        Use immutable classes or return copies.

    Avoid Shared Static Fields:

        Avoid modifying static variables in instance methods; make them thread-safe if necessary.

    Input Validation (Manual Review Suggestion):

        Validate and sanitize all user input before processing to prevent injection attacks.

7️⃣ Remediation Plan
Vulnerability	Remediation Action	Example Fix
Default Encoding	Define UTF-8 charset explicitly.	new InputStreamReader(file, StandardCharsets.UTF_8)
Resource Leak	Close streams automatically.	try (FileInputStream fis = new FileInputStream(file)) {}
Mutable Exposure	Return defensive copies.	return Arrays.copyOf(data, data.length);
Static Field Modification	Redesign shared variables.	Convert to instance variables or synchronize access.
8️⃣ Results Summary
Metric	Result
Total Classes Scanned	~270
Total Issues Found	6
High Severity	2
Medium Severity	4
Report Generated	✅ spotbugsXml.xml
Tool Used	SpotBugs (Maven Plugin)
9️⃣ Conclusion

The OWASP WebGoat Java application was successfully analyzed using SpotBugs.
The review revealed several common security weaknesses, such as improper encoding handling, resource management flaws, mutable object exposure, and unsafe static field operations.

Implementing the recommended fixes will significantly improve the application’s code security, stability, and compliance with OWASP secure coding standards.

This exercise demonstrates the effectiveness of static analysis tools in finding security issues early in the development lifecycle and reinforces the importance of secure coding practices in software engineering.
🔧 Tools Summary
Tool	Purpose
Java (OpenJDK 17)	Programming language
Maven	Build and dependency manager
SpotBugs	Static code analysis
Kali Linux	Environment for testing and development
GitHub	Documentation and sharing of results
✍️ Author

Name: Faaiz Khan
Task: Secure Coding Review (OWASP WebGoat – Java)
Environment: Kali Linux
Tools: Java | Maven | SpotBugs | OWASP WebGoat
Keywords: #CyberSecurity #SecureCoding #OWASP #SpotBugs #JavaSecurity #StaticAnalysis
