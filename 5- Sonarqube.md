##  DAY-5 | SONARQUBE  ##
SonarQube is an open-source platform that provides static code analysis and code quality management. It is designed to help developers and development teams identify and fix code issues early in the software development lifecycle. SonarQube analyzes source code for bugs, vulnerabilities, code smells, and code duplications, and provides detailed reports with actionable insights.



Here are some key features of SonarQube:

- **Static Code Analysis**: SonarQube performs static analysis on source code, analyzing its structure, syntax, and patterns to identify potential issues and violations of coding standards.

- **Code Quality Metrics**: SonarQube calculates various code quality metrics, such as code coverage, cyclomatic complexity, duplications, and maintainability index. These metrics help developers evaluate the overall quality of their codebase.

- **Issue Tracking**: SonarQube identifies and categorizes issues in the code, such as bugs, security vulnerabilities, and code smells. It provides detailed information about each issue, including the affected code snippet, severity level, and recommended fixes.

- **Continuous Inspection**: SonarQube supports continuous integration and continuous delivery (CI/CD) workflows by integrating with popular build tools and version control systems. It can be seamlessly integrated into the development pipeline to automatically analyze code with every build.

- **Custom Rules and Quality Profiles**: SonarQube allows you to define custom coding rules and quality profiles to align with your project's specific requirements and coding standards. This helps enforce consistent code quality across your development team.

- **Integrations**: SonarQube integrates with a wide range of development tools and IDEs, enabling developers to receive real-time feedback on code quality while they write code. It also integrates with popular CI/CD platforms to provide continuous inspection throughout the software development process.

By using SonarQube, development teams can proactively identify and address code issues, improve code maintainability, enhance security, and ensure adherence to coding standards. It helps foster a culture of quality within development teams and promotes the delivery of robust and reliable software.
```
--- Intalação alterativa ---
# Guia de Instalação e Uso do SonarQube no WSL2 (Ubuntu)

Este guia documenta os passos para instalar e operar o SonarQube 8.9.10 LTS diretamente no Ubuntu (rodando no WSL2), sem o uso de Docker. Este método foi validado após troubleshooting que indicou incompatibilidade da imagem Docker do SonarQube com o ambiente WSL2 específico.

---

## Como Ligar o SonarQube (Rotina Diária)

Use este checklist toda vez que você ligar seu notebook e precisar iniciar o serviço do SonarQube.

### Passo 1: Iniciar o Ubuntu (WSL)
Abra seu terminal do Ubuntu a partir do Menu Iniciar do Windows.

### Passo 2: Aplicar o Pré-requisito do Kernel
Esta configuração é essencial para o Elasticsearch (componente do SonarQube) e precisa ser aplicada a cada reinicialização do WSL, a menos que tenha sido tornada permanente.

```bash
sudo sysctl -w vm.max_map_count=262144
```

> **⭐ Dica de Ouro: Tornando a Configuração Permanente**
> Para não precisar digitar o comando acima toda vez, execute o comando abaixo **apenas uma vez**. Ele adicionará a configuração aos arquivos de inicialização do sistema.
> ```bash
> echo "vm.max_map_count=262144" | sudo tee -a /etc/sysctl.conf
> ```

### Passo 3: Iniciar o Servidor SonarQube
**Importante:** Execute estes comandos como seu usuário normal (ex: `devops`), **NÃO** como `root`.

```bash
# Navega para o diretório de scripts do SonarQube
cd ~/sonarqube-8.9.10.61524/bin/linux-x86-64/

# Inicia o servidor em segundo plano
./sonar.sh start
```

### Passo 4: Aguardar e Acessar
1.  **Aguarde pacientemente de 2 a 3 minutos.** O SonarQube é uma aplicação pesada e precisa desse tempo para carregar completamente.
2.  (Opcional) Monitore a inicialização em tempo real para confirmar que tudo está ocorrendo bem:
    ```bash
    tail -f ~/sonarqube-8.9.10.61524/logs/sonar.log
    ```
    Espere até ver a mensagem `--> SonarQube is up`. Pressione `Ctrl+C` para sair do monitoramento.
3.  Pegue o endereço IP do seu WSL para o acesso:
    ```bash
    ip addr show eth0
    ```
4.  Acesse no seu navegador do Windows (a porta padrão é **9000**):
    `http://<IP_DO_SEU_WSL>:9000`

---

## Como Instalar o SonarQube do Zero (Guia Completo)

Use este guia para uma instalação limpa em um novo ambiente Ubuntu 22.04 no WSL2.

### Passo 1: Instalar Pré-requisitos
Instale o Java 11 (requerido pelo SonarQube 8.9) e outras ferramentas úteis.

```bash
sudo apt-get update && sudo apt-get install -y openjdk-11-jdk wget unzip
```

### Passo 2: Configurar o Kernel Permanentemente
Aplique a configuração `vm.max_map_count` e já a salve para futuras reinicializações.

```bash
# Aplica para a sessão atual
sudo sysctl -w vm.max_map_count=262144

# Torna a configuração permanente
echo "vm.max_map_count=262144" | sudo tee -a /etc/sysctl.conf
```

### Passo 3: Baixar e Descompactar o SonarQube
Vamos baixar e extrair os arquivos do SonarQube 8.9.10 LTS.

```bash
# Vá para sua pasta de usuário
cd ~

# Baixe o arquivo zip
wget [https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-8.9.10.61524.zip](https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-8.9.10.61524.zip)

# Descompacte
unzip sonarqube-8.9.10.61524.zip
```

### Passo 4: Definir Permissões
É crucial que seu usuário seja o dono da pasta do SonarQube para evitar erros de permissão, pois o serviço não pode ser executado como `root`.

```bash
# O $USER será substituído pelo seu nome de usuário atual (ex: devops)
sudo chown -R $USER:$USER ~/sonarqube-8.9.10.61524
```

### Passo 5: Iniciar e Verificar
Agora, inicie o serviço pela primeira vez.

1.  **Inicie o servidor (como seu usuário, NÃO como root):**
    ```bash
    cd ~/sonarqube-8.9.10.61524/bin/linux-x86-64/
    ./sonar.sh start
    ```
2.  **Aguarde** de 2 a 3 minutos.
3.  **Verifique os logs** para confirmar que ele subiu com a mensagem `--> SonarQube is up`:
    ```bash
    tail -f ~/sonarqube-8.9.10.61524/logs/sonar.log
    ```
4.  **Pegue o IP** com `ip addr show eth0` e acesse `http://<IP_DO_SEU_WSL>:9000` no navegador.

--- Docker Installation ---

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg

sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo apt install docker-compose

service docker restart
sudo usermod -aG docker $USER
newgrp docker
sudo chmod 666 /var/run/docker.sock
sudo systemctl restart docker


# TO ISNTALL SONARQUBE USING DOCKER RUN BELOW COMMAND  
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community

```

## Real World Scenario: Code Quality Assessment and Continuous Improvement

### Background
A software development team is working on a complex web application project with multiple developers contributing code. The team wants to ensure code quality and identify and address any code issues early in the development process. They decide to incorporate SonarQube into their workflow to achieve these goals.

### Steps

### 1. **Integration with CI/CD Pipeline:**  
   The team integrates SonarQube into their CI/CD pipeline, ensuring that code analysis is performed automatically with every build. They configure their build tool to trigger SonarQube analysis after the code compilation step.

### 2. **Static Code Analysis:**  
   During each build, SonarQube performs static code analysis on the source code. It scans the codebase for bugs, vulnerabilities, code smells, and code duplications.

### 3. **Quality Reports and Dashboards:**  
   SonarQube generates detailed quality reports and provides a dashboard that highlights code issues, quality metrics, and trends over time. The team can easily access these reports to gain insights into the codebase's overall quality and track improvements.

### 4. **Issue Management:**  
   SonarQube categorizes code issues by severity and provides detailed information about each issue. The team can prioritize and address critical issues promptly to prevent potential bugs and security vulnerabilities.

### 5. **Code Review and Collaboration:**  
   SonarQube facilitates code review and collaboration within the team. Developers can review the SonarQube reports, identify areas for improvement, and discuss solutions to address code issues.

### 6. **Enforcement of Coding Standards:**  
   SonarQube enforces coding standards by checking code against predefined rules and quality profiles. The team configures SonarQube to match their project's specific requirements and coding standards, ensuring consistent code quality across the development team.

### 7. **Continuous Improvement:**  
   With SonarQube's insights and reports, the team continuously improves the code quality. They identify recurring issues, refactor code, and apply best practices to prevent similar issues in the future.

### 8. **Monitoring Code Quality Trends:**  
   The team regularly monitors code quality trends using SonarQube's reports and dashboards. They can identify improvements or regressions in code quality over time and take appropriate actions to maintain or enhance the overall quality.

By incorporating SonarQube into their development workflow, the team can proactively identify and address code issues, improve code maintainability, enhance security, and adhere to coding standards. SonarQube helps the team foster a culture of quality and continuous improvement, leading to the delivery of robust and reliable software.


### To make sure code coverage works fine add below things in your pom file at respective places or use my Repo

Here's the POM format with the added items for enabling code coverage with JaCoCo:

```xml
<project>
    <!-- Other project configuration -->

    <properties>
        <!-- JaCoCo Properties -->
        <jacoco.version>0.8.6</jacoco.version>
        <sonar.java.coveragePlugin>jacoco</sonar.java.coveragePlugin>
        <sonar.dynamicAnalysis>reuseReports</sonar.dynamicAnalysis>
        <sonar.jacoco.reportPath>${project.basedir}/../target/jacoco.exec</sonar.jacoco.reportPath>
        <sonar.language>java</sonar.language>
    </properties>

    <!-- Other dependencies -->

    <dependencies>
        <!-- Other dependencies -->

        <dependency>
            <groupId>org.jacoco</groupId>
            <artifactId>jacoco-maven-plugin</artifactId>
            <version>0.8.6</version>
        </dependency>

        <!-- Other dependencies -->
    </dependencies>

    <!-- Other plugin configurations -->

    <build>
        <plugins>
            <!-- Other plugins -->

            <plugin>
                <groupId>org.jacoco</groupId>
                <artifactId>jacoco-maven-plugin</artifactId>
                <version>${jacoco.version}</version>
                <executions>
                    <execution>
                        <id>jacoco-initialize</id>
                        <goals>
                            <goal>prepare-agent</goal>
                        </goals>
                    </execution>
                    <execution>
                        <id>jacoco-site</id>
                        <phase>package</phase>
                        <goals>
                            <goal>report</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

            <!-- Other plugins -->
        </plugins>
    </build>

    <!-- Other project configuration -->
</project>
```

Make sure to include this code snippet within the `<project>` tags of your existing POM file, and adjust any other project-specific configurations as needed.

### Jenkins pipeline to run SOnar analysis

```powershell
pipeline {
    agent any
    
    tools{
        jdk 'jdk11'
        maven 'maven3'
    }
    
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/silviojpa/Petclinic.git'
            }
        }
        
        stage("Compile"){
            steps{
                sh "mvn clean compile"
            }
        }
        
        stage("Test Cases"){
            steps{
                sh "mvn test"
            }
        }
        
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Petclinic \
                    -Dsonar.java.binaries=. \
                    -Dsonar.projectKey=Petclinic '''
    
                }
            }
        }
        
         stage("Built"){
            steps{
                sh "mvn clean package -DskipTests=true"
            }
        }
        
        stage("Deploy"){
            steps{
                sh "cp target/petclinic.war /opt/apache-tomcat-9.0.65/webapps"
            }
        }
    }
}
```

```powershell
pipeline {
    agent any
    
    tools {
        jdk 'jdk11'
        maven 'maven3'
    }
    
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
    
    stages {
        stage('Checkout') {
            steps {
                # Checkout your Java project from version control
                # For example:
                # git 'https://github.com/your-repo/java-project.git'
            }
        }
        
        stage('Build') {
            steps {
                # Build your Java project
                # For example:
                # sh 'mvn clean install'
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                # Run SonarQube analysis
                # Make sure you have SonarQube configured in Jenkins and provide the correct SonarQube server credentials
                
                
                
                # Or using the SonarQube Scanner for Maven:
                # sh 'mvn sonar:sonar'
            }
        }
        
        stage('Deploy to Tomcat') {
            steps {
                # Additional steps to deploy your Java project
                Invoke-Expression "sudo cp target/*war apache-tomcat-path/webapps"
            }
        }
    }
}
```
### Once the pipeline is success you will get the results in sonarqube as below.

![alt text](https://github.com/jaiswaladi246/30-Days-Of-DevOps/blob/main/Images/3.png?raw=true)
