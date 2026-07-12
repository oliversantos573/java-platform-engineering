# Java Platform Engineering

<div align="center">

# Module 01 - JVM Foundations

## Lesson 01 - O Nascimento de um Programa Java

Aprenda o que realmente acontece desde o momento em que um arquivo `.java` é compilado até a JVM executar o método `main()`.

---

**Nível:** Fundamentos da Plataforma Java

**Status:** ✅ Concluído

**JDK:** 21 LTS

</div>

---

# Objetivos

Ao final desta aula você será capaz de:

- Entender o processo de compilação Java.
- Compreender o que é um arquivo `.class`.
- Entender o conceito de Bytecode.
- Utilizar o comando `javap`.
- Analisar a estrutura interna de uma classe compilada.
- Compreender o papel do Constant Pool.
- Entender como a JVM identifica uma classe.

---

# Fluxo Geral

```text
             Código Fonte

           HelloJVM.java
                 │
                 │ javac
                 ▼
           HelloJVM.class
                 │
                 │ Class Loader
                 ▼
               JVM
                 │
                 ▼
         Execution Engine
                 │
                 ▼
               CPU
```

---

# O Código Fonte

```java
package com.oliversantos.engineering.module01.startup;

public class HelloJVM {

    public static void main(String[] args) {

        System.out.println("==================================");
        System.out.println(" Java Platform Engineering");
        System.out.println(" Module 01 - JVM Foundations");

    }

}
```

---

# Compilação

Compilar o projeto:

```bash
mvn clean compile
```

Após a compilação será gerado:

```text
target/classes/com/oliversantos/engineering/module01/startup/HelloJVM.class
```

---

# Inspecionando o Bytecode

Executar:

```bash
javap -verbose target/classes/com/oliversantos/engineering/module01/startup/HelloJVM.class
```

---

# Primeira Descoberta

A JVM **não executa código Java**.

Ela executa **Bytecode**.

O compilador (`javac`) transforma o código fonte em um conjunto de instruções padronizadas chamadas Bytecodes.

Essas instruções são independentes do sistema operacional e podem ser executadas por qualquer JVM compatível.

---

# Estrutura do Arquivo .class

Durante a análise encontramos:

```text
minor version: 0
major version: 65
```

## O que significa?

A versão do Bytecode.

| Java | Major Version |
|------|---------------|
| Java 8 | 52 |
| Java 11 | 55 |
| Java 17 | 61 |
| Java 21 | 65 |

Isso explica erros como:

```
UnsupportedClassVersionError
```

A JVM utilizada é incapaz de interpretar aquela versão do Bytecode.

---

# Herança Implícita

Encontramos:

```text
super_class:
java/lang/Object
```

Mesmo escrevendo:

```java
public class HelloJVM {

}
```

O compilador gera:

```java
public class HelloJVM extends Object {

}
```

Toda classe Java herda de `java.lang.Object`.

---

# Quantidade de Métodos

Encontramos:

```text
methods: 2
```

Embora tenhamos escrito apenas:

```java
main()
```

Existe também:

```java
HelloJVM()
```

O compilador cria automaticamente um construtor padrão quando nenhum construtor é declarado.

---

# Constant Pool

A seção mais importante do arquivo `.class`.

```text
Constant pool:
```

O Constant Pool funciona como uma tabela contendo todas as constantes utilizadas pela classe.

Exemplos encontrados:

- Strings
- Métodos
- Classes
- Campos
- Assinaturas
- Tipos
- Literais

Exemplo:

```text
#13 = String
==================================
```

```text
#21 = String
Java Platform Engineering
```

```text
#25 = Class
HelloJVM
```

O Bytecode referencia essas posições durante a execução.

---

# Bytecode Gerado

```text
0: getstatic
3: ldc
5: invokevirtual

8: getstatic
11: ldc
13: invokevirtual

16: getstatic
19: ldc
21: invokevirtual

24: return
```

---

# Entendendo Cada Instrução

## getstatic

Obtém um campo estático.

No nosso exemplo:

```java
System.out
```

---

## ldc

Load Constant.

Carrega uma constante do Constant Pool.

Exemplo:

```java
"Java Platform Engineering"
```

---

## invokevirtual

Invoca um método de instância.

No nosso caso:

```java
println()
```

---

## return

Finaliza a execução do método.

---

# Como a JVM interpreta

O código:

```java
System.out.println("Hello");
```

É transformado em:

```text
getstatic
ldc
invokevirtual
```

A JVM interpreta essas instruções uma a uma.

---

# O que aprendemos

Nesta aula aprendemos que:

- Java é compilado para Bytecode.
- A JVM executa Bytecode e não código Java.
- Um arquivo `.class` é um arquivo binário padronizado.
- O Constant Pool armazena todas as referências necessárias da classe.
- Toda classe herda de `Object`.
- O compilador gera automaticamente um construtor padrão.
- O comando `javap` permite inspecionar um arquivo `.class`.

---

# Exercícios

## Exercício 01

Explique com suas palavras:

> O que é um arquivo `.class`?

---

## Exercício 02

Explique:

> O que é Bytecode?

---

## Exercício 03

O que representa:

```text
major version: 65
```

---

## Exercício 04

Por que aparecem dois métodos no arquivo compilado?

---

## Exercício 05

Explique o que é o Constant Pool.

---

## Exercício 06

Explique o funcionamento das instruções:

- getstatic
- ldc
- invokevirtual
- return

---

# Desafio

Explique detalhadamente todo o fluxo abaixo.

```text
HelloJVM.java

↓

javac

↓

HelloJVM.class

↓

ClassLoader

↓

JVM

↓

Execution Engine

↓

CPU
```

---

# Como um Engenheiro da Plataforma Java pensa

Um desenvolvedor normalmente diz:

> "A JVM executa meu código Java."

Um engenheiro da plataforma Java diz:

> "O compilador transforma o código-fonte em um arquivo `.class` contendo bytecode e metadados. Durante a execução, a JVM carrega esse arquivo, verifica sua integridade, resolve referências através do Constant Pool e interpreta ou compila o bytecode para código nativo antes da execução."

Essa diferença de entendimento é o objetivo deste treinamento.

---

# Referências Oficiais

## Especificações da Plataforma Java

- 📘 **Java Virtual Machine Specification (JVMS)**
  https://docs.oracle.com/javase/specs/jvms/se21/html/

- 📘 **Java Language Specification (JLS)**
  https://docs.oracle.com/javase/specs/jls/se21/html/

---

## Documentação Oficial

- ☕ **Oracle Java Documentation**
  https://docs.oracle.com/en/java/javase/21/

- ☕ **OpenJDK**
  https://openjdk.org/

- ☕ **OpenJDK JDK 21**
  https://openjdk.org/projects/jdk/21/

---

## Ferramentas Utilizadas

### javac

https://docs.oracle.com/en/java/javase/21/docs/specs/man/javac.html

### javap

https://docs.oracle.com/en/java/javase/21/docs/specs/man/javap.html

### java

https://docs.oracle.com/en/java/javase/21/docs/specs/man/java.html

---

## APIs Oficiais

Java SE 21 API

https://docs.oracle.com/en/java/javase/21/docs/api/

---

## Código Fonte da Plataforma Java

OpenJDK no GitHub

https://github.com/openjdk/jdk

---

## Material Complementar

Inside the Java (Blog Oficial da Oracle)

https://inside.java/

Foojay

https://foojay.io/

Baeldung

https://www.baeldung.com/

---
# 📖 Leitura Obrigatória

Antes de iniciar a próxima aula, leia:

## JVMS

Capítulo 4 — The class File Format

https://docs.oracle.com/javase/specs/jvms/se21/html/jvms-4.html

Tempo estimado: 30 minutos.

Objetivo:

- Entender a estrutura de um arquivo `.class`.
- Conhecer o Constant Pool.
- Compreender os metadados presentes no bytecode.

## Livro Recomendado

The Java® Virtual Machine Specification

https://docs.oracle.com/javase/specs/


# Próxima Aula

## Lesson 02 — Stack Frames e Operand Stack

Na próxima aula aprenderemos:

- Como a JVM executa bytecodes.
- O que significa `stack=2`.
- O que significa `locals=1`.
- Stack Frames.
- Operand Stack.
- Local Variable Table.
- Como cada instrução manipula a pilha da JVM.

---

<div align="center">

**Java Platform Engineering**

**Module 01 • Lesson 01**

Construindo conhecimento profundo sobre a Plataforma Java.

</div>