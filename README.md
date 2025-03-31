# Manual de SQL

Este manual fornece uma introdução básica ao SQL, incluindo conceitos fundamentais, tipos de dados, comandos comuns e como realizar operações de banco de dados em PostgreSQL utilizando Java.

---

## Conceitos Básicos

### Campo
Um campo é uma única unidade de dados dentro de uma tabela. Também é conhecido como coluna. Cada campo tem um nome e um tipo de dado associado (como `INTEGER`, `VARCHAR`, etc.).

### Tabela
Uma tabela é uma estrutura onde os dados são armazenados. Ela é composta por linhas (também chamadas de registros) e colunas (campos).

### Linha
Uma linha é uma entrada completa em uma tabela. Cada linha armazena dados específicos para uma entidade.

### Coluna
Uma coluna é um conjunto de campos com o mesmo tipo de dado em uma tabela. As colunas definem os atributos das informações armazenadas.

---

## Tipos de Dados no Banco de Dados

Os tipos de dados em SQL especificam o tipo de valor que cada coluna pode armazenar. Alguns dos tipos mais comuns são:

- **`INT`**: Armazena números inteiros.
- **`VARCHAR(n)`**: Armazena texto com comprimento variável, até `n` caracteres.
- **`DATE`**: Armazena datas no formato `YYYY-MM-DD`.
- **`BOOLEAN`**: Armazena valores `TRUE` ou `FALSE`.
- **`DECIMAL(p, s)`**: Armazena números decimais com precisão `p` e escala `s`.

---

## Conexão em Java com PostgreSQL

Para se conectar ao PostgreSQL em Java, é necessário incluir o driver JDBC do PostgreSQL no seu projeto. Este é o trecho necessário no arquivo `pom.xml` para usar o PostgreSQL no Maven.

### Arquivo `pom.xml`

```xml
<dependencies>
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <version>42.2.23</version> <!-- Certifique-se de usar a versão mais recente -->
    </dependency>
</dependencies>
```

### Código de Conexão

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DatabaseConnection {
    public static Connection connect() throws SQLException {
        String url = "jdbc:postgresql://localhost:5432/seubanco";
        String user = "seuusuario";
        String password = "suasenha";
        return DriverManager.getConnection(url, user, password);
    }
}
```

---

## Comandos SQL

### Comando `CREATE DATABASE`

Cria um novo banco de dados no PostgreSQL.

```sql
CREATE DATABASE nome_do_banco;
```

### Comando `DROP DATABASE`

Exclui um banco de dados existente.

```sql
DROP DATABASE nome_do_banco;
```

### Comando `CREATE TABLE`

Cria uma nova tabela em um banco de dados.

```sql
CREATE TABLE nome_da_tabela (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100),
    idade INT
);
```

### Comando `DROP TABLE`

Exclui uma tabela do banco de dados.

```sql
DROP TABLE nome_da_tabela;
```

### Comando `TRUNCATE TABLE`

Remove todos os dados de uma tabela, mas mantém a estrutura da tabela.

```sql
TRUNCATE TABLE nome_da_tabela;
```

---

## Execução dos Comandos no PostgreSQL com Java

### Código para Criar Banco de Dados

```java
import java.sql.Statement;
import java.sql.Connection;

public class CreateDatabase {
    public static void createDatabase() {
        try (Connection connection = DatabaseConnection.connect();
             Statement statement = connection.createStatement()) {
            String sql = "CREATE DATABASE nome_do_banco";
            statement.executeUpdate(sql);
            System.out.println("Banco de dados criado com sucesso!");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### Código para Criar Tabela

```java
public class CreateTable {
    public static void createTable() {
        String createTableSQL = "CREATE TABLE clientes (" +
                                "id SERIAL PRIMARY KEY, " +
                                "nome VARCHAR(100), " +
                                "email VARCHAR(100))";
        try (Connection connection = DatabaseConnection.connect();
             Statement statement = connection.createStatement()) {
            statement.executeUpdate(createTableSQL);
            System.out.println("Tabela criada com sucesso!");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

---

## Comando `INSERT`

Insere dados em uma tabela.

```sql
INSERT INTO clientes (nome, email) VALUES ('João', 'joao@exemplo.com');
```

### Execução do Comando `INSERT` em Java

```java
import java.sql.PreparedStatement;

public class InsertData {
    public static void insertData() {
        String insertSQL = "INSERT INTO clientes (nome, email) VALUES (?, ?)";
        try (Connection connection = DatabaseConnection.connect();
             PreparedStatement preparedStatement = connection.prepareStatement(insertSQL)) {
            preparedStatement.setString(1, "Maria");
            preparedStatement.setString(2, "maria@exemplo.com");
            preparedStatement.executeUpdate();
            System.out.println("Dados inseridos com sucesso!");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

---

## Comando `SELECT`

Seleciona dados de uma tabela.

```sql
SELECT * FROM clientes;
```

## Comando `DELETE`
Exemplo de DELETE
//...
try {
    int id = 1; 
    String comando = "DELETE FROM veiculos WHERE id = ?";
    PreparedStatement pstmt;
    pstmt = conexao.prepareStatement(comando);
    pstmt.setInt(1, id);
    pstmt.executeUpdate();
    JOptionPane.showMessageDialog(this, "Apagado com sucesso");
} catch (Exception e) {
    JOptionPane.showMessageDialog(this, "Erro ao apagar");
}
exemplo 2

try {
    String id_ = jLabel1.getText();
    int id = Integer.parseInt(id_);
    String comando = "DELETE FROM veiculos WHERE id = ?";
    PreparedStatement pstmt;
    pstmt = conexao.prepareStatement(comando);
    pstmt.setInt(1, id);
    pstmt.executeUpdate();
    JOptionPane.showMessageDialog(this, "Apagado com sucesso");
    jButton5.doClick();
} catch (Exception e) {
    JOptionPane.showMessageDialog(this, "Erro ao apagar");
}

### Execução do Comando `SELECT` em Java

```java
import java.sql.ResultSet;
import java.sql.Statement;

public class SelectData {
    public static void selectData() {
        String selectSQL = "SELECT * FROM clientes";
        try (Connection connection = DatabaseConnection.connect();
             Statement statement = connection.createStatement();
             ResultSet resultSet = statement.executeQuery(selectSQL)) {
            
            while (resultSet.next()) {
                int id = resultSet.getInt("id");
                String nome = resultSet.getString("nome");
                String email = resultSet.getString("email");
                System.out.println(id + " - " + nome + " - " + email);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

---

## Apresentação dos Dados no Console

Os dados recuperados da consulta SQL podem ser exibidos diretamente no console com um loop sobre o `ResultSet`, como mostrado no exemplo acima.

---

## Exemplo de `JTable` no Java

O `JTable` é usado para exibir dados em uma tabela gráfica em Java.

### Código para Exibir Dados em um `JTable`

```java
import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import java.sql.*;

public class DisplayInJTable {
    public static void displayData() {
        String[] columnNames = {"ID", "Nome", "Email"};
        DefaultTableModel model = new DefaultTableModel(columnNames, 0);
        
        String selectSQL = "SELECT * FROM clientes";
        try (Connection connection = DatabaseConnection.connect();
             Statement statement = connection.createStatement();
             ResultSet resultSet = statement.executeQuery(selectSQL)) {
            
            while (resultSet.next()) {
                int id = resultSet.getInt("id");
                String nome = resultSet.getString("nome");
                String email = resultSet.getString("email");
                model.addRow(new Object[]{id, nome, email});
            }
            
            JTable table = new JTable(model);
            JOptionPane.showMessageDialog(null, new JScrollPane(table), "Dados dos Clientes", JOptionPane.INFORMATION_MESSAGE);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

---

