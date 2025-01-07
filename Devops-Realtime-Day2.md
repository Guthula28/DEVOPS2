## How to work as a Devops Engineer
* As a devops engineer, we are supposed to build and package the code, this is very much technology dependant.
    * Java Packages: Contain compiled bytecode files (.class) that the Java Virtual Machine (JVM) can run.
    * .NET Packages: Contain Intermediate Language (IL) code that the .NET runtime compiles to native code at execution time.
    * C/C++: Produce executable binaries directly from source code that can run natively on the target system.
    * Node.js/Python/Shell Scripts: Involve copying source code files to the target system where they are interpreted or executed directly.

* Yes, as a DevOps engineer, the approach to building and packaging code can vary significantly depending on the technology stack you're working with. Here’s a quick overview of how this process can differ based on the technology:

1. **Java**: 
   - **Build Tools**: Use tools like Maven or Gradle.
   - **Packaging**: Create `.jar` (Java ARchive) files or `.war` (Web Application Archive) files.
   - **Process**: Source code is compiled into bytecode (`.class` files), then bundled into packages that the JVM can execute.

2. **.NET**:
   - **Build Tools**: Use MSBuild or the .NET CLI.
   - **Packaging**: Create `.dll` (Dynamic Link Library) files or `.exe` (executable) files.
   - **Process**: Source code is compiled into Intermediate Language (IL), which is then compiled to native code at runtime by the .NET runtime.

3. **C/C++**:
   - **Build Tools**: Use tools like `make`, `cmake`, or integrated development environments (IDEs) with build capabilities.
   - **Packaging**: Create native binaries (`.exe` for Windows or executables for Unix-based systems) that can be run directly.
   - **Process**: Source code is compiled into machine code directly executable on the target system.

4. **Node.js**:
   - **Build Tools**: Use tools like npm or yarn for dependency management.
   - **Packaging**: Generally, no formal packaging; just bundle dependencies and source code.
   - **Process**: Deploy source code and dependencies; Node.js runs the JavaScript directly in its runtime environment.

5. **Python**:
   - **Build Tools**: Use tools like `setuptools` or `pip` for packaging.
   - **Packaging**: Create source distributions or wheel files (`.tar.gz`, `.whl`).
   - **Process**: Deploy source code and dependencies; Python interprets and runs the code directly.

6. **Shell Scripts**:
   - **Build Tools**: Generally not applicable as they are interpreted directly.
   - **Packaging**: Distribute script files directly.
   - **Process**: Copy scripts to the target system where they are executed by the shell.

* In summary, the build and packaging process is highly technology-dependent and aligns with how each environment handles code execution, whether through bytecode, intermediate code, or direct execution.
  
* Java and .NET applications typically require compilation due to the nature of the languages and their respective runtimes, whereas languages like Python and Node.js are interpreted, allowing them to run without a formal compilation step. Here's why:

### 1. **Java and .NET (Compiled Languages)**
   - **Java** and **.NET (C#, VB.NET)** are compiled to an intermediate bytecode (Java bytecode for Java and CIL/MSIL for .NET). These bytecodes are then executed by a virtual machine, such as the Java Virtual Machine (JVM) for Java or the Common Language Runtime (CLR) for .NET.
   - **Reasons for Compilation**:
     - **Performance**: Compiled code is generally faster than interpreted code. The virtual machine (JVM or CLR) can optimize bytecode during execution (just-in-time compilation), making it more efficient.
     - **Static Typing**: Both Java and .NET are statically typed languages, meaning type checking is done at compile time, reducing the chances of runtime errors.
     - **Cross-platform Support**: Java and .NET programs are compiled into platform-independent bytecode, which can then be run on any machine with the appropriate virtual machine.

### 2. **Python and Node.js (Interpreted Languages)**
   - **Python** and **JavaScript (Node.js)** are interpreted languages, which means that their code is executed line-by-line by an interpreter (Python interpreter or the V8 engine for Node.js).
   - **No Compilation**:
     - **Dynamic Typing**: Python and JavaScript are dynamically typed, so types are resolved at runtime, eliminating the need for a compilation step.
     - **Flexibility**: Interpreted languages allow for rapid development and prototyping because you don’t have to wait for the code to be compiled. This is especially useful in environments where quick iteration is needed.
     - **Scripting**: These languages are often used for scripting and lightweight applications, where execution speed may be less critical than development speed.

### Trade-offs:
   - **Compiled languages** (Java, .NET) tend to offer better runtime performance, security, and type safety but require an additional compilation step before running.
   - **Interpreted languages** (Python, Node.js) offer faster development cycles and flexibility but may have slower execution times, and runtime errors may only be caught during execution.

* In modern development, the choice between compiling and interpreting largely depends on the nature of the project, performance needs, and developer preference.
  
## Build Tools

* Certainly! Here’s a more comprehensive overview including Apache Maven:

1. **Make**:
   - **Purpose**: Automates the build process for C and C++ projects.
   - **Configuration**: Uses a file called `Makefile` to specify build instructions, including compilation steps, dependencies, and targets.
   - **Usage**: Commonly used in Unix-like systems. The `Makefile` defines rules and commands that `make` executes to compile and link the code into executables or libraries.

2. **Apache Ant**:
   - **Purpose**: Automates the build process for Java projects and other languages.
   - **Configuration**: Uses a file called `build.xml` to define build tasks, such as compilation, packaging, and deployment.
   - **Usage**: XML-based configuration allows for a flexible and extensible build process. Ant can handle complex build requirements and integrate with other tools.

3. **Apache Maven**:
   - **Purpose**: A build tool primarily used for Java projects and also supports Java-based languages like Groovy and Scala.
   - **Configuration**: Relies on conventions and uses a file called `pom.xml` (Project Object Model) to define build steps and dependencies.
   - **Features**: Maven supports building, testing, packaging, and managing dependencies with a focus on convention over configuration, simplifying the build process by providing a standard structure and predefined lifecycle phases.
   - Maven has a direcotory layout [Refer Here](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html)

   - Maven includes a feature called **archetypes** that helps with project setup. Here’s how it fits into the build process:

   - **Purpose**: A build tool primarily used for Java projects and supports Java-based languages like Groovy and Scala.
   - **Configuration**: Uses a file called `pom.xml` (Project Object Model) to define build steps, dependencies, and project configuration.
   - **Features**:
   - **Build Management**: Supports building, testing, packaging, and deploying applications.
   - **Dependency Management**: Manages project dependencies through a centralized repository.
   - **Convention over Configuration**: Provides a standardized project structure and predefined build lifecycle phases to simplify configuration.
   - **Archetypes**: Maven archetypes are project templates that generate a project’s directory structure and sample code. They provide a quick way to set up new projects with a predefined structure, making it easier to start with best practices and conventions. For example, using an archetype, you can generate a basic Maven project with sample code, ready to be customized for your needs.

  - Archetypes can be customized or created to suit specific project needs, making Maven a powerful tool for both managing and initializing projects.

## Setup Maven

* Apache Maven relies on Java to function, so Java needs to be installed on your system for Maven to work properly. Here’s a quick summary of the prerequisites and setup:

## Maven Setup Prerequisites:
1. Java Installation:

    * Requirement: Maven requires a Java Development Kit (JDK) to be installed on your system.
    * Version: Ensure you have a compatible JDK version installed.Maven generally supports various JDK versions, but it's good to check Maven’s documentation for specific version requirements.

2. Setting JAVA_HOME:

    * Configuration: You need to set the JAVA_HOME environment variable to point to the directory where the JDK is installed. This helps Maven locate the Java runtime environment it needs to execute.

3. Maven Installation:

    * Download and Setup: Download Maven from the official Apache Maven website and extract it to a directory.
    * Configuration: Set the M2_HOME environment variable to the Maven installation directory, and add Maven’s bin directory to the system PATH so you can run Maven commands from the command line.

4. Verification:

    * Check Installation: After setting up, you can verify the installation by running mvn -version in your command line or terminal. This command should display the installed versions of Maven and Java.

## Steps for Installation:
* Install Java

```
sudo apt update && sudo apt install openjdk-17-jdk -y 
----------------------Or -------------------------
sudo apt update
sudo apt-cache search jdk
sudo apt install openjdk-17-jdk -y
```
* maven (tar based installed): Lets download maven from [Refer Here](https://maven.apache.org/download.cgi)
```
cd /tmp
wget https://dlcdn.apache.org/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz
tar -xvf apache-maven-3.9.6-bin.tar.gz
sudo mv apache-maven-3.9.6 /opt/maven
```

  
* ADD opt/maven/bin to PATH in ``` /etc/environment ```

```
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/opt/maven/bin"
```
* exit and relogin


* Execute ``` mvn -version ``` command

## To remove OpenJDK (the one you've already installed)

```
update-alternatives --list java
sudo apt-get purge openjdk-\* -y
sudo apt autoremove
#relogin
```
## Project Object Model(POM)
* [Refer Here](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html)  for official docs
* Simple pom.xml

```xml
<project>
       <modelVersion>4.0.0</modelVersion>
       <groupId>blog.devopseasy</groupId>
       <artifactId>hello-maven</artifactId>
       <version>0.0.1-SNAPSHOT</version>
</project
```

* Apache Maven uses a well-defined lifecycle to manage the build process of a project. The Maven build lifecycle consists of a sequence of phases that handle various stages of a project’s build process. Each phase represents a specific step in the build cycle, and Maven executes these phases in a predefined order.

![Preview](./Images/maven-lifecycle.png)

### **Maven Build Lifecycle**

* Maven has three built-in lifecycles: **default**, **clean**, and **site**. Each lifecycle consists of a series of phases, and each phase is associated with specific goals or tasks.

#### 1. **Default Lifecycle**
* The default lifecycle handles the build and deployment of the project. It is the most commonly used lifecycle and includes the following phases:

1. **validate**: Validate the project to ensure all necessary information is available.
2. **compile**: Compile the source code of the project.
3. **test**: Run tests using a suitable testing framework (e.g., JUnit) to verify the compiled code.
4. **package**: Package the compiled code into a distributable format, such as a JAR, WAR, or ZIP file.
5. **verify**: Perform any additional verification to ensure the quality of the package (e.g., integration tests).
6. **install**: Install the package into the local Maven repository, making it available for other projects on the same machine.
7. **deploy**: Deploy the package to a remote repository so that other developers or projects can access it.

#### 2. **Clean Lifecycle**
* The clean lifecycle handles the cleaning up of files and directories that were created during the build process. It includes the following phases:

1. **pre-clean**: Perform any necessary setup before cleaning.
2. **clean**: Remove all files generated by the previous build.
3. **post-clean**: Perform any necessary actions after cleaning.

#### 3. **Site Lifecycle**
The site lifecycle is used for generating project documentation. It includes the following phases:

1. **pre-site**: Perform any setup required before generating the site.
2. **site**: Generate the project’s site documentation.
3. **post-site**: Perform any actions required after generating the site.
4. **site-deploy**: Deploy the generated site documentation to a web server or repository.

### **Lifecycle Execution**

* When you run a Maven command, it triggers the execution of phases within these lifecycles. For example, if you run `mvn package`, Maven will execute the phases from `validate` to `package` in the default lifecycle. If any phase fails, Maven will stop the build process and report the error.

### **Example of Maven Lifecycle Execution**

* If you execute the command:

```sh
mvn install
```

* Maven will run the following phases in the default lifecycle:

1. **validate**
2. **compile**
3. **test**
4. **package**
5. **verify**
6. **install**

* It will not run the `deploy` phase as it is not explicitly called.

* Understanding the Maven lifecycle helps in configuring and managing the build process, ensuring that each phase is executed in the correct order and that the build process is efficient and reliable.


### **Maven Artifacts and Directories**

1. **Target Directory**:
   - **Location**: Maven creates a directory called `target` in the root of your project.
   - **Purpose**: This directory is used to store the compiled code, packaged artifacts (such as JAR, WAR, or ZIP files), and other build outputs. The `target` directory is automatically generated and managed by Maven.
   - **Clean Step**: When you run the `mvn clean` command, Maven deletes the `target` directory to remove all the files generated by previous builds, ensuring a fresh build environment.

2. **Local Repository (`.m2` Directory)**:
   - **Location**: When Maven is installed, it creates a `.m2` directory in your user’s home directory (`C:\Users\<username>\.m2` on Windows or `/home/<username>/.m2` on Unix-based systems).
   - **Purpose**: The `.m2` directory serves as Maven’s local repository. It contains downloaded dependencies, plugins, and other artifacts required for your projects. The local repository caches artifacts to avoid repeated downloads from remote repositories.
   - **Repository Structure**: Within `.m2`, the `repository` subdirectory is where Maven stores all the cached dependencies and artifacts organized by group ID, artifact ID, and version.

### **Maven Repositories**

1. **Local Repository**:
   - **Description**: The local repository is located in the `.m2` directory and is used by Maven to store artifacts locally on your machine. When Maven builds a project, it first checks this repository for required dependencies before looking in remote repositories.

2. **Remote Repositories**:
   - **Description**: Remote repositories are external locations from which Maven can download dependencies and plugins. These repositories can be:
     - **Public Repositories**: Hosted by third parties, such as Maven Central or JCenter, which are commonly used by developers to find and download open-source libraries.
     - **Private Repositories**: Hosted by organizations or teams, often to store proprietary artifacts or internal libraries. These can be managed using tools like Nexus or Artifactory.

3. **Central Repository**:
   - **Description**: The Maven Central Repository is the default remote repository used by Maven. It is a large, public repository that hosts a vast collection of open-source Java libraries and other artifacts. Maven searches this repository to resolve dependencies if they are not found in the local repository.

### **Summary**

- **`target` Directory**: Contains the results of the build process and is deleted by the `clean` phase.
- **`.m2` Directory**: Contains the local repository where Maven stores cached dependencies and artifacts.
- **Repositories**:
  - **Local**: Located in the `.m2` directory.
  - **Remote**: External repositories where Maven fetches dependencies not found locally.
  - **Central**: The default public repository that Maven uses to retrieve a wide range of open-source libraries.

* These components are integral to Maven’s ability to manage project builds, dependencies, and artifacts effectively.

![Preview](./Images/Maven-Repository.jpg)

## Building sample projects 

* Maven involves creating and managing a basic project to understand how Maven operates. Here’s a step-by-step guide to setting up and building sample Maven projects:

### **1. Setting Up Maven**

* Before you build a sample project, ensure Maven is installed and properly configured:

1. **Download Maven**:
   - Visit the [Apache Maven download page](https://maven.apache.org/download.cgi) and download the binary zip or tar.gz file.

2. **Install Maven**:
   - Extract the downloaded file to a directory (e.g., `C:\apache-maven-3.8.4` on Windows or `/usr/local/apache-maven-3.8.4` on Unix-based systems).
   - Set the `M2_HOME` environment variable to point to the Maven directory.
   - Add Maven’s `bin` directory to the system `PATH`.

3. **Verify Installation**:
   - Open a terminal or command prompt and run `mvn -version` to check that Maven is installed correctly.

### **2. Creating a Sample Maven Project**

You can use Maven’s built-in archetypes to generate a sample project with a standard structure:

1. **Generate a Project Using an Archetype**:
   - Open a terminal or command prompt.
   - Run the following command to generate a sample project:
     ```sh
     mvn archetype:generate -DgroupId=com.example -DartifactId=my-sample-project -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
     ```
   - **Explanation**:
     - `-DgroupId=com.example`: The group ID for your project (often represents the company or organization).
     - `-DartifactId=my-sample-project`: The artifact ID for your project (usually represents the project name).
     - `-DarchetypeArtifactId=maven-archetype-quickstart`: Specifies the archetype to use, which in this case is a basic Java project.
     - `-DinteractiveMode=false`: Runs the command without prompting for user input.

2. **Navigate to the Project Directory**:
   - Change into the newly created project directory:
     ```sh
     cd my-sample-project
     ```

3. **Project Structure**:
   - The generated project will have a structure like this:
     ```
     my-sample-project
     ├── pom.xml
     └── src
         ├── main
         │   └── java
         │       └── com
         │           └── example
         │               └── App.java
         └── test
             └── java
                 └── com
                     └── example
                         └── AppTest.java
     ```

4. **Understanding `pom.xml`**:
   - The `pom.xml` file is the core configuration file for Maven. It defines project dependencies, plugins, and build configuration. Here’s a basic example of a `pom.xml` file:
     ```xml
     <project xmlns="http://maven.apache.org/POM/4.0.0"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/POM/4.0.0">
         <modelVersion>4.0.0</modelVersion>
         <groupId>com.example</groupId>
         <artifactId>my-sample-project</artifactId>
         <version>1.0-SNAPSHOT</version>
         <dependencies>
             <dependency>
                 <groupId>junit</groupId>
                 <artifactId>junit</artifactId>
                 <version>4.12</version>
                 <scope>test</scope>
             </dependency>
         </dependencies>
     </project>
     ```

### **3. Building the Project**

1. **Compile the Project**:
   - Run the following command to compile the source code:
     ```sh
     mvn compile
     ```

2. **Run Tests**:
   - Run the following command to execute the unit tests:
     ```sh
     mvn test
     ```

3. **Package the Project**:
   - Package the compiled code into a JAR file using the command:
     ```sh
     mvn package
     ```
   - After packaging, the resulting JAR file will be located in the `target` directory.

4. **Install the Project**:
   - Install the project into your local Maven repository with:
     ```sh
     mvn install
     ```
   - This makes the project’s artifacts available for use by other projects on your local machine.

### **4. Running the Sample Application**

* If you want to run the sample Java application created by the archetype, you can use the `java` command to execute the JAR file:

1. **Find the JAR File**:
   - The JAR file will be located in the `target` directory, typically named `my-sample-project-1.0-SNAPSHOT.jar`.

2. **Run the JAR File**:
   - Use the following command to run the JAR file:
     ```sh
     java -cp target/my-sample-project-1.0-SNAPSHOT.jar com.example.App
     ```

   - This command runs the `App` class specified in the `com.example` package.

### **Summary**

* By following these steps, you can set up, build, and run a sample Maven project. This process introduces you to the basic Maven commands and project structure, allowing you to start managing more complex projects and dependencies.
* 
## To remove OpenJDK (the one you've already installed)

```
update-alternatives --list java
sudo apt-get purge openjdk-\* -y
sudo apt autoremove
#relogin
```
## 1. Building one more opensource project
* we have installed jdk 17
* Lets build [Refer Here](https://github.com/jenkins-docs/simple-java-maven-app)

```
git clone https://github.com/jenkins-docs/simple-java-maven-app.git
cd simple-java-maven-app
mvn validate
mvn compile
mvn test
mvn package
```

## 2. Building springpetclinic project
*  This project requires java 17 

* [Refer Here](https://github.com/spring-projects/spring-petclinic)
```
git clone https://github.com/spring-projects/spring-petclinic.git
cd spring-petclinic
mvn clean package
```

## 3. Building game-of-life 
* This project requires java 8
* [Refer Here](https://github.com/wakaleo/game-of-life)

```
git clone https://github.com/wakaleo/game-of-life.git
cd game-of-life
mvn clean package
```
## 4. Building Broadleaf ecommerce
* This project requires java 11

```
git clone https://github.com/BroadleafCommerce/BroadleafCommerce
cd BroadleafCommerce
mvn clean package

```

## 5. Building Openmrs
* This project requires java 11

```
git clone https://github.com/openmrs/openmrs-core.git
cd openmrs-core
mvn clean package

```

## 6. Building Spring Boot REST API
* This project requires java 21

```
git clone https://github.com/spring-petclinic/spring-petclinic-rest.git
cd spring-petclinic-rest/
mvn clean package
```

* Absolutely! Here’s a more detailed explanation based on your outline:

### **Libraries**

* Libraries are collections of pre-written code that can be reused by multiple programs or projects. They help in achieving common functionality without needing to reinvent the wheel. Libraries are typically categorized into two main types:

1. **Static Libraries**:
   - **Definition**: Static libraries are collections of object files that are linked into an application at compile time.
   - **Characteristics**:
     - The code from static libraries is copied into the final executable.
     - The application does not need the static library at runtime, as all necessary code is already included in the executable.
     - Example File Extensions: `.a` (Unix), `.lib` (Windows).
   - **Advantages**:
     - No need for the library to be present on the target system.
     - Faster runtime performance since all code is included.
   - **Disadvantages**:
     - Larger executable size.
     - Updates to the library require recompiling the application.

2. **Dynamic Libraries**:
   - **Definition**: Dynamic libraries are linked to an application at runtime rather than at compile time.
   - **Characteristics**:
     - The code from dynamic libraries is not included in the executable but is loaded into memory when the application starts or when needed.
     - The application relies on the library being present on the system where it runs.
     - Example File Extensions: `.so` (Unix), `.dll` (Windows), `.dylib` (macOS).
   - **Advantages**:
     - Smaller executable size.
     - Libraries can be updated independently of the application.
   - **Disadvantages**:
     - The application must ensure that the correct version of the library is present on the target system.
     - Potential issues with version compatibility or missing libraries at runtime.

### **Dependencies**

* Dependencies refer to the libraries or modules that a project relies on to function properly. These dependencies provide additional functionality and are integrated at the source code level.

- **Dependency Management**: In modern development, managing dependencies is crucial for ensuring that projects have access to the necessary libraries and that versions are consistent. Tools help manage these dependencies efficiently.
  
### **Modern Build Tools**

* Modern build tools address various aspects of project management and development. They provide functionalities that simplify and automate tasks related to building, testing, and managing dependencies. Key features include:

1. **Building Code**:
   - **Purpose**: Automate the process of compiling source code, linking libraries, and packaging the final application.
   - **Examples**: Maven (Java), Gradle (Java, Groovy, Kotlin), Make (C/C++).

2. **Dependency Management**:
   - **Purpose**: Handle the inclusion of external libraries and modules, ensuring that the correct versions are used and resolving any conflicts.
   - **Examples**: Maven and Gradle for Java, npm for Node.js, pip for Python.

3. **Artifact Versioning**:
   - **Purpose**: Manage different versions of libraries or modules to ensure compatibility and track changes over time.
   - **Examples**: Maven Central, npm Registry, PyPI.

4. **Storage Support**:
   - **Purpose**: Provide repositories for storing and retrieving artifacts, including both the project’s own artifacts and third-party dependencies.
   - **Examples**: Maven Repository (for Java artifacts), npm Registry (for JavaScript packages), PyPI (for Python packages).

### **Summary**

- **Libraries**: Reusable code that can be static or dynamic, each with its own advantages and disadvantages.
- **Dependencies**: External libraries or modules that a project relies on for functionality.
- **Modern Build Tools**: Tools that automate the build process, manage dependencies, handle artifact versioning, and provide storage support to streamline development workflows.

## Multi-project model in Maven
* In these cases, we would have parent pom and each subdirectory will have pom files
* [Refer Here](https://github.com/wakaleo/game-of-life) to understand the multiproject models
* In Maven, the multi-project model allows you to manage multiple projects within a single parent project. This approach is useful for complex applications that consist of multiple modules or components, enabling better organization, dependency management, and build coordination.

### **Key Concepts of Maven Multi-Project Model**

1. **Parent POM (Project Object Model)**:
   - The parent `pom.xml` file defines common configuration, dependency management, and plugin settings for all child projects (modules).
   - It provides a centralized configuration, making it easier to manage and update dependencies, plugin versions, and other build settings across all modules.

2. **Child Modules**:
   - Each child module is a separate Maven project with its own `pom.xml` file.
   - Child modules inherit configuration from the parent POM but can also have their own specific settings and dependencies.
   - Modules are defined in the parent POM using the `<modules>` section.

3. **Multi-Module Project Structure**:
   - The project structure typically looks like this:
     ```
     parent-project
     ├── pom.xml (Parent POM)
     ├── module-a
     │   └── pom.xml
     ├── module-b
     │   └── pom.xml
     └── module-c
         └── pom.xml
     ```
   - In this structure:
     - `parent-project` is the root directory containing the parent POM.
     - `module-a`, `module-b`, and `module-c` are child modules, each with its own `pom.xml`.

### **Configuring a Multi-Project Build**

1. **Create the Parent POM**:
   - Define the parent POM (`parent-project/pom.xml`) with common settings and module definitions:
     ```xml
     <project xmlns="http://maven.apache.org/POM/4.0.0"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/POM/4.0.0">
         <modelVersion>4.0.0</modelVersion>
         <groupId>com.example</groupId>
         <artifactId>parent-project</artifactId>
         <version>1.0-SNAPSHOT</version>
         <packaging>pom</packaging>
         <modules>
             <module>module-a</module>
             <module>module-b</module>
             <module>module-c</module>
         </modules>
         <dependencyManagement>
             <dependencies>
                 <!-- Define shared dependencies with versions -->
             </dependencies>
         </dependencyManagement>
         <build>
             <pluginManagement>
                 <plugins>
                     <!-- Define shared plugins and versions -->
                 </plugins>
             </pluginManagement>
         </build>
     </project>
     ```

2. **Create Child Modules**:
   - Define each child module with its own `pom.xml`, inheriting from the parent POM:
     ```xml
     <project xmlns="http://maven.apache.org/POM/4.0.0"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/POM/4.0.0">
         <modelVersion>4.0.0</modelVersion>
         <parent>
             <groupId>com.example</groupId>
             <artifactId>parent-project</artifactId>
             <version>1.0-SNAPSHOT</version>
         </parent>
         <artifactId>module-a</artifactId>
         <!-- Module-specific configuration -->
     </project>
     ```

### **Benefits of Multi-Project Model**

1. **Centralized Management**:
   - Shared configuration and dependency versions are managed in the parent POM, reducing duplication and ensuring consistency across modules.

2. **Modularization**:
   - Projects are organized into discrete modules, promoting separation of concerns and making it easier to manage complex applications.

3. **Build Coordination**:
   - The parent POM allows for coordinated builds, ensuring that all modules are built in the correct order, respecting their interdependencies.

4. **Inheritance and Overriding**:
   - Child modules inherit common settings from the parent POM but can override specific configurations or dependencies as needed.

5. **Efficient Dependency Management**:
   - Shared dependencies and plugin versions are managed centrally, making updates and version control more straightforward.

### **Running Builds**

* To build the entire multi-project setup, you can run Maven commands from the parent project directory. Maven will handle the build process for all modules in the specified order:

- **Clean and Build All Modules**:
  ```sh
  mvn clean install
  ```

- **Build a Specific Module**:
  Navigate to the module directory and run:
  ```sh
  mvn clean install
  ```

* Using the multi-project model in Maven helps manage complex applications by leveraging modularization, centralized configuration, and coordinated builds.

* The Maven Wrapper (`mvnw`) is a tool provided by Apache Maven that allows you to execute Maven builds without requiring the user to install Maven separately on their system. It ensures that the project is built with the specific Maven version that the project was designed to work with. Here’s a detailed overview of the Maven Wrapper:

### **What is the Maven Wrapper?**
* [Refer Here](https://maven.apache.org/wrapper/)
* The Maven Wrapper consists of a set of scripts (`mvnw` for Unix-based systems and `mvnw.cmd` for Windows) and configuration files that automate the process of downloading and using the correct version of Maven for your project. This ensures consistency across different environments and simplifies the setup for new contributors.

### **Key Components**

1. **Wrapper Scripts**:
   - **Unix-based Systems**: `mvnw` (Bash script)
   - **Windows**: `mvnw.cmd` (Batch script)

2. **Configuration Files**:
   - **`.mvn/wrapper/maven-wrapper.properties`**: Contains configuration for the Maven Wrapper, including the Maven distribution URL.
   - **`.mvn/wrapper/maven-wrapper.jar`**: The JAR file used by the wrapper scripts to download and run the specified Maven version.

### **Setting Up the Maven Wrapper**

* To add the Maven Wrapper to your project, follow these steps:

1. **Generate Wrapper Files**:
   - Run the following command in the root of your Maven project:
     ```sh
     mvn -N io.takari:maven:wrapper
     ```
   - This command will generate the necessary wrapper files and directories in your project.

2. **Verify Wrapper Files**:
   - After running the command, you should see the following files and directories:
     ```
     .mvn/
     └── wrapper/
         ├── maven-wrapper.jar
         └── maven-wrapper.properties
     mvnw
     mvnw.cmd
     ```

3. **Configure Wrapper Properties**:
   - The `maven-wrapper.properties` file inside the `.mvn/wrapper` directory contains configuration for the Maven Wrapper. You can specify the desired Maven version and distribution URL:
     ```properties
     distributionUrl=https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/3.8.6/apache-maven-3.8.6-bin.zip
     ```

### **Using the Maven Wrapper**

* Once the Maven Wrapper is set up, you can use it to execute Maven commands without needing to have Maven installed globally. Here’s how you use it:

1. **Run Maven Commands**:
   - **Unix-based Systems**: Use the `mvnw` script.
     ```sh
     ./mvnw clean install
     ```
   - **Windows**: Use the `mvnw.cmd` script.
     ```sh
     mvnw.cmd clean install
     ```

2. **Automatic Download**:
   - When you run `mvnw` or `mvnw.cmd`, the wrapper will automatically download and use the correct version of Maven as specified in the `maven-wrapper.properties` file.

### **Benefits of Using the Maven Wrapper**

1. **Consistency**:
   - Ensures that all developers and CI/CD environments use the same Maven version, avoiding issues caused by version discrepancies.

2. **Ease of Use**:
   - New contributors can build the project without needing to install Maven manually, simplifying the setup process.

3. **Version Control**:
   - The wrapper scripts and configuration files can be version-controlled alongside your project, ensuring that the correct Maven version is used consistently.

4. **Isolation**:
   - Avoids conflicts with system-installed Maven versions and configurations, providing a clean and isolated build environment.

### **Summary**

* The Maven Wrapper (`mvnw`) is a powerful tool that helps manage Maven versions consistently across different environments. By including the wrapper in your project, you ensure that everyone working on the project uses the same Maven version, reducing setup issues and build inconsistencies.

## Maven Test Results
* Generally maven stores test results in ``` target/surefire-reports/*.xml ```
* In Maven, testing is an integral part of the build process, and managing test results effectively is crucial for maintaining code quality. Maven provides built-in support for running and reporting unit tests and integration tests, primarily through plugins such as the **Surefire Plugin** and **Failsafe Plugin**. Here's how Maven handles test results:

### **1. Test Execution**

- **Unit Tests**: Typically run during the `test` phase of the Maven build lifecycle using the Surefire Plugin.
- **Integration Tests**: Run during the `verify` phase using the Failsafe Plugin.

### **2. Surefire Plugin**

* The Surefire Plugin is used for running unit tests. It is configured by default in Maven and performs the following:

- **Default Configuration**: Runs tests in the `src/test/java` directory.
- **Test Classes**: By default, it looks for test classes with names matching `*Test.java`, `*Tests.java`, or `*TestCase.java`.
- **Test Results Location**: Results are stored in the `target/surefire-reports` directory.

#### **Configuring Surefire Plugin**

* In the `pom.xml` file, you can configure the Surefire Plugin to customize test execution and reporting:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>3.0.0-M5</version>
            <configuration>
                <includes>
                    <include>**/*Test.java</include>
                </includes>
                <testFailureIgnore>true</testFailureIgnore>
            </configuration>
        </plugin>
    </plugins>
</build>
```

### **3. Failsafe Plugin**

* The Failsafe Plugin is used for running integration tests, which are typically more complex and require an environment similar to production. It runs during the `verify` phase.

- **Default Configuration**: Runs tests in the `src/test/java` directory, but focuses on integration tests.
- **Test Classes**: By default, it looks for test classes with names matching `*IT.java` or `*ITCase.java`.
- **Test Results Location**: Results are stored in the `target/failsafe-reports` directory.

#### **Configuring Failsafe Plugin**

* In the `pom.xml` file, you can configure the Failsafe Plugin to customize integration test execution:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-failsafe-plugin</artifactId>
            <version>3.0.0-M5</version>
            <executions>
                <execution>
                    <goals>
                        <goal>integration-test</goal>
                        <goal>verify</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

### **4. Test Results Reporting**

- **Surefire Reports**: Generated in `target/surefire-reports` and include files like `TEST-*.xml`, which contain detailed test results.
- **Failsafe Reports**: Generated in `target/failsafe-reports` and include files like `TEST-*.xml`, which contain detailed integration test results.

### **5. Viewing Test Results**

- **Command Line**: Test results are typically available in the console output of the Maven build. Maven provides summary reports after tests are run.
- **HTML Reports**: Surefire and Failsafe Plugins generate HTML reports that are more user-friendly. They are located in `target/surefire-reports/index.html` and `target/failsafe-reports/index.html`, respectively.

### **6. Sample Test Result Files**

* Here’s what typical test result files look like:

- **XML Files**: Test result files in `target/surefire-reports` or `target/failsafe-reports` contain XML-formatted results, which can be parsed by continuous integration tools or build servers.
  ```xml
  <testsuite name="com.example.MyTest" tests="3" failures="1" errors="0" skipped="0">
      <testcase classname="com.example.MyTest" name="testMethod1" time="0.002"/>
      <testcase classname="com.example.MyTest" name="testMethod2" time="0.001">
          <failure message="Expected value but got different." type="java.lang.AssertionError">
              junit.framework.ComparisonFailure: Expected value but got different.
          </failure>
      </testcase>
  </testsuite>
  ```

### **7. Continuous Integration (CI) Integration**

* Maven’s test result reports are compatible with many CI/CD systems such as Jenkins, GitHub Actions, and GitLab CI. These tools can be configured to display test results, track trends, and provide detailed feedback on test failures and coverage.

### **Summary**

- **Surefire Plugin**: Handles unit tests, with results in `target/surefire-reports`.
- **Failsafe Plugin**: Handles integration tests, with results in `target/failsafe-reports`.
- **Test Results**: Available in XML format and can be viewed as HTML reports. Configuration allows customization of test execution and reporting.
- **CI/CD Integration**: Test results can be integrated with continuous integration systems for automated feedback and reporting.

* Using Maven’s testing capabilities and the results they generate helps maintain high code quality and reliability throughout the development lifecycle.

## Sample Java projects to try building
Here are some GitHub repositories with sample Java projects that you can explore, clone, and build. These repositories cover a range of complexities, from basic to advanced, and include various aspects of Java development.




### Projects with Frameworks**

1. **Spring Boot REST API**
   - **Repository**: [Spring Boot REST API Example](https://github.com/spring-petclinic/spring-petclinic-rest)
   - **Description**: A RESTful API built with Spring Boot.

2. **OpenMRS**
   - **Repository**: [Refer Here](https://github.com/openmrs/openmrs-core?tab=readme-ov-file#prerequisites)
   - **Description**: OpenMRS is a patient-based medical record system
3. **Broadleaf ecommerce**
   - **Repository**: [Refer Here](https://github.com/BroadleafCommerce/BroadleafCommerce)
   - **Description**: Broadleaf Commerce CE is an e-commerce framework written entirely in Java and leveraging the Spring framework.
   
### **How to Clone and Build a Project**

1. **Clone the Repository**:
   ```sh
   git clone <repository-url>
   ```

2. **Navigate to the Project Directory**:
   ```sh
   cd <project-directory>
   ```

3. **Build the Project**:
   - If the project uses Maven:
     ```sh
     mvn clean install
     ```
   - If the project uses Gradle:
     ```sh
     gradle build
     ```

4. **Run the Application**:
   - For console applications, you may run:
     ```sh
     java -cp target/<jar-file>.jar com.example.MainClass
     ```
   - For web applications, you might deploy it to a server like Tomcat or run it directly if it's a Spring Boot application:
     ```sh
     mvn spring-boot:run
     ```

* These repositories provide a good mix of project types and complexities, allowing you to practice and enhance your Java development skills.

## Building dotnet core projects
* dotnet uses a inhouse tool called as msbuild to build the projects
* In dotnet core msbuild is the integral part of sdk installation
* dotnet generally has build configurations
    * Debug dotnet build -c Debug <path to csproj>
    * Release dotnet build -c Release <path to csproj>
* using dotnet command
    * dotnet build: this command compiles the code and generates dlls
    * dotnet test: this command executes the tests and generates test results
    * dotnet publish: this command creates the complete project build output (artifact) into a folderwhich you can further zip and share.
    * To run the application we can execute dotnet <app-main>.dll
    
    ## nopcommerce
    * [Refer Here](https://github.com/nopSolutions/nopCommerce) for projects github
    * Ensure dotnet 8 sdk is installed [Refer Here](https://learn.microsoft.com/en-us/dotnet/core/install/linux-ubuntu-install?pivots=os-linux-ubuntu-2204&tabs=dotnet8#ubuntu-2204)

    ```
    sudo apt-get update && sudo apt-get install -y dotnet-sdk-8.0
    ```
    * clone the project and build
    ```
    git clone https://github.com/nopSolutions/nopCommerce.git
    cd nopCommerce
    cd src/Presentation/Nop.Web
    dotnet build -c Release Nop.Web.csproj
    mkdir ~/publish 
    dotnet publish -c Release Nop.Web.csproj -o ~/publish
    cd ../../Tests/Nop.Tests
    dotnet test Nop.Tests.csproj
   ```

