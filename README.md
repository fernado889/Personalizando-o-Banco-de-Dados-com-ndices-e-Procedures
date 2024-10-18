# Personalizando-o-Banco-de-Dados-com-ndices-e-Procedures
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class CriarIndice {
    public static void main(String[] args) {
        // Conectar ao banco de dados
        String url = "jdbc:mysql://localhost:3306/meubanco";
        String usuario = "meuusuario";
        String senha = "minhasenha";

        try (Connection conn = DriverManager.getConnection(url, usuario, senha)) {
            // Criar um statement
            Statement stmt = conn.createStatement();

            // Criar um índice na tabela "clientes"
            String sql = "CREATE INDEX idx_nome ON clientes (nome)";
            stmt.executeUpdate(sql);

            System.out.println("Índice criado com sucesso!");
        } catch (SQLException e) {
            System.out.println("Erro ao criar índice: " + e.getMessage());
        }
    }
}
import java.sql.CallableStatement;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class CriarProcedure {
    public static void main(String[] args) {
        // Conectar ao banco de dados
        String url = "jdbc:mysql://localhost:3306/meubanco";
        String usuario = "meuusuario";
        String senha = "minhasenha";

        try (Connection conn = DriverManager.getConnection(url, usuario, senha)) {
            // Criar um callable statement
            String sql = "CREATE PROCEDURE sp_buscar_clientes(IN nome VARCHAR(50)) BEGIN SELECT * FROM clientes WHERE nome LIKE CONCAT('%', nome, '%'); END";
            CallableStatement cstmt = conn.prepareCall(sql);

            // Executar a procedure
            cstmt.executeUpdate();

            System.out.println("Procedure criada com sucesso!");
        } catch (SQLException e) {
            System.out.println("Erro ao criar procedure: " + e.getMessage());
        }
    }
}
import java.sql.CallableStatement;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;

public class ChamarProcedure {
    public static void main(String[] args) {
        // Conectar ao banco de dados
        String url = "jdbc:mysql://localhost:3306/meubanco";
        String usuario = "meuusuario";
        String senha = "minhasenha";

        try (Connection conn = DriverManager.getConnection(url, usuario, senha)) {
            // Criar um callable statement
            String sql = "CALL sp_buscar_clientes(?)";
            CallableStatement cstmt = conn.prepareCall(sql);

            // Definir o parâmetro
            cstmt.setString(1, "João");

            // Executar a procedure
            ResultSet rs = cstmt.executeQuery();

            // Imprimir os resultados
            while (rs.next()) {
                System.out.println(rs.getString("nome") + " - " + rs.getString("email"));
            }
        } catch (SQLException e) {
            System.out.println("Erro ao chamar procedure: " + e.getMessage());
        }
    }
